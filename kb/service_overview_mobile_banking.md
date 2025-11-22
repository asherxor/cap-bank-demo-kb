# Service Overview – Mobile Banking App

## 1. Purpose

The mobile banking app is the bank’s primary **customer-facing digital channel** for:

- Checking balances and transactions.
- Initiating payments and transfers.
- Managing cards (freeze/unfreeze, limits).
- Receiving security notifications and alerts.

It is treated as a **Tier-1, SEV-1 critical service** in all availability and security planning.

---

## 2. High-Level Architecture

- **Client:** Native iOS and Android apps.
- **API Gateway:** HTTPS REST API exposed via `api.bank.example`.
- **Backend Services:**
  - Auth service (OIDC / OAuth2).
  - Accounts service (balance, transaction history).
  - Payments service (internal & external transfers).
  - Notifications service (push + SMS).
- **Core Banking Integration:**
  - ESB or service connector to main core banking system.
  - All monetary transactions ultimately settled in core banking.

---

## 3. Security & Compliance Expectations

- All traffic over TLS 1.2+.
- Strong customer authentication (SCA) where required.
- Device binding and session management:
  - Short-lived access tokens.
  - Refresh tokens stored securely.
- Regulatory alignment:
  - NIST CSF 2.0 for security posture.
  - ISO 27001 for ISMS controls.
  - Where applicable: PCI-DSS for card data, HIPAA only if health-related products exist (low likelihood).

---

## 4. Deployment & Release

- CI/CD pipeline builds mobile backend images.
- SBOM generated at build time and ingested into **CAP** via `/v1/sbom/ingest`.
- Each release is:
  - Tied to a **Subject** in CAP (`service:mobile-banking`).
  - Covered by an **Attestation** that CAP can verify and anchor to Canon.

Key requirements:

1. **No production deployment** without:
   - Fresh SBOM.
   - Successful attestation verification.
2. **Change management policy** must be followed (see `policies/change_management_policy.md`).

---

## 5. Operational Dependencies

- Identity provider (IdP) / SSO.
- Logging & monitoring stack (APM, central logs, metrics).
- CAP core backend available for:
  - Evidence ingestion.
  - Attestation verification and anchoring.

---

## 6. Known Risks (for TTX / CAP Demo)

- Outage or major slowdown during peak hours.
- Integration failures between mobile API and core banking.
- Disinformation campaigns claiming:
  - Funds are “missing”.
  - The bank has been hacked.
  - The app is unsafe or compromised.

These are explicitly used in the **Bank / Alto demo scenarios**.
