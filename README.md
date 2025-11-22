# CAP Platform Backend (Rounds #1–2)

## Quick Start (Windows PowerShell)

```powershell
# 1. Python env (3.11+)
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r core\requirements.txt -c core\constraints.txt

# 2. Node deps for gateway
cd gateway
pnpm install
cd ..

# 3. Apply DB migrations (0001 + 0002)
cd db
..\.\.venv\Scripts\python.exe -m alembic upgrade head
cd ..

# 4. Seed frameworks + local pref profile
.\.venv\Scripts\python.exe scripts/seed_frameworks.py
$env:DATABASE_URL='postgres://postgres:200200@localhost:5432/cap'
.\.venv\Scripts\python.exe scripts/seed_locked_local_prefs.py

# 5. Run services (keep both windows open)
pwsh -File scripts/run.core.win.ps1
pwsh -File scripts/run.gateway.win.ps1
```

## Local-Only Anchoring Mode

Round #2 introduces tenant-scoped preferences and local anchoring providers (Canon + Local Merkle).  
Global flags are set in `.env`:

```
CAP_CANON_LOCAL_ENABLED=true
CAP_LOCAL_MERKLE_ENABLED=true
CAP_SCITT_ENABLED=false
CAP_ANCHOR_DEFAULT_STRATEGY=local_only
```

### Tenant Preferences API

```powershell
$token = (Invoke-RestMethod -Method Post `
  -Uri http://localhost:4000/auth/login `
  -ContentType 'application/json' `
  -Body '{"email":"admin@cap.local","password":"pass"}').accessToken
$tenant = "aed71ab5-e01f-4f42-bbc8-338bf545754d"
$headers = @{ Authorization = "Bearer $token"; "x-tenant-id" = $tenant }

# Read locked-local profile
Invoke-RestMethod -Uri "http://127.0.0.1:8100/v1/tenants/$tenant/preferences" -Headers $headers

# Update (example toggling local_merkle requirement)
$prefs = Invoke-RestMethod -Uri "http://127.0.0.1:8100/v1/tenants/$tenant/preferences" -Headers $headers
$prefs.anchoring.providers.local_merkle.required = $true
Invoke-RestMethod -Method Put -Uri "http://127.0.0.1:8100/v1/tenants/$tenant/preferences" `
  -Headers $headers -ContentType 'application/json' -Body ($prefs | ConvertTo-Json -Depth 10)
```

### Anchoring Flow (Round #2 smoke)

Assuming an attestation is already `verified`:

```powershell
# Anchor using locked-local routing (canon_local only by default)
$anchor = Invoke-RestMethod -Method Post `
  -Uri "http://127.0.0.1:8100/v1/attestations/$($attestationId)/anchor" `
  -Headers $headers

# Inspect anchor receipt
$anchor

# Verify proof for a specific anchor id
Invoke-RestMethod -Uri "http://127.0.0.1:8100/v1/anchors/$($anchor.anchors[0].anchor_id)/verify" -Headers $headers
```

## Round #1 Core Smoke (Auth + SBOM/DBOM/Evidence)

```powershell
# Health
Invoke-RestMethod http://127.0.0.1:8100/health

# Login
$login = Invoke-RestMethod -Method Post `
  -Uri http://localhost:4000/auth/login `
  -ContentType 'application/json' `
  -Body '{"email":"admin@cap.local","password":"pass"}'
$token  = $login.accessToken
$tenant = $login.user.tenant.id
$headers = @{ Authorization = "Bearer $token"; "x-tenant-id" = $tenant }

# Evidence upload
Invoke-RestMethod -Method Post `
  -Uri http://127.0.0.1:8100/v1/evidence `
  -Headers $headers -ContentType 'application/json' `
  -Body '{"filename":"demo.txt","mime":"text/plain","content_base64":"SGVsbG8gQ0FQ"}'

# SBOM ingest
Invoke-RestMethod -Method Post `
  -Uri http://127.0.0.1:8100/v1/sbom/ingest `
  -Headers $headers -ContentType 'application/json' `
  -Body '{"format":"spdx","build_digest":"sha256:1111...","raw_json":{"SPDXID":"SPDXRef-Demo","spdxVersion":"SPDX-2.3"}}'

# DBOM ingest
Invoke-RestMethod -Method Post `
  -Uri http://127.0.0.1:8100/v1/dbom/ingest `
  -Headers $headers -ContentType 'application/json' `
  -Body '{"raw":"key: value\ncomponents:\n  - name: demo","source_uri":"file://demo.yml"}'
```

All commands above are auditable—copy JSON responses into your ops log (e.g., `docs/round1_smoke.md`) before moving to the next round.
# cap-bank-demo-kb
