# Phase 6 – Automation, QA, and UX Polish (Work Log)

## Scope
- Close gaps from previous phases: automated tests (sync/index/agents), QA on Agents UI/flows, and ops polish.
- Improve semantic search/indexing readiness (document placeholder vs. future vector index).
- Harden sync + audit observability; validate seeds after resets.
- UX polish: status badges (sync/index), loading/error states, and smart-fill UX feedback.

## Plan
1) Testing & QA
   - Backend tests: sync status transitions (pending→synced/failed), index_stale hooks, agent run/promote audits, semantic search endpoint.
   - Frontend smoke: login → dashboard → agents list/create/run → sync now → evidence upload shows sync metadata.
   - Document test results in Execution Log and Issues.
2) Sync & Observability
   - Ensure sync endpoint logs/audits with counts and errors (done in Phase 4; verify via tests).
   - Expose sync/index status in API responses where missing (artifacts/snapshots/evidence already partially done).
   - Add minimal health/status endpoint or log markers if needed.
3) Semantic Search readiness
   - Keep SQL ILIKE fallback; document requirement for vector index for future improvement.
   - Optional guardrail: limit search length, return top-N with artifact context.
4) UX polish
   - Agents screen: loading/error states, status badges for sync, tooltip for templates, disable run when no agent selected.
   - Surface sync metadata badges in Evidence/Knowledge lists (if time permits).
5) Seed & Ops
   - Ensure reset scripts run migrations + seed tenant/admin/plan/supervisor/personal agents; note no extra seed needed.
   - Document reset+seed steps.

## Execution Log
- [x] Backend test coverage added (sync metadata on snapshots + search fallback; agent run audit)
- [x] Backend tests executed (TEST_DATABASE_URL=postgresql+asyncpg://postgres:200200@localhost:5432/cap_test) → PASSED
- [ ] Frontend smoke executed (login → dashboard → agents run → sync) *(manual step pending)*
- [ ] Sync observability verified (audit + status in responses) *(backend ok; UI partial)*
- [x] UX polish applied (sync badges on Evidence table)
- [x] Seed/reset steps documented (see docs/ops/reset_seed.md)
- [ ] Issues encountered: (to fill)
- [ ] Status/outcome: (to fill)

## Issues & Notes (to fill during execution)
- Keep seeds after resets (tenant/admin/plan/agents). Use scripts/reset_db.win.ps1 + seed_admin_and_agents.win.ps1; see docs/ops/reset_seed.md.
- Semantic search still SQL placeholder; vector index deferred.
- Tests require async driver: set TEST_DATABASE_URL to postgresql+asyncpg://... (psycopg2 fails with async engine).
- Frontend smoke not run yet (manual step needed); sync observability on UI still basic (only evidence table shows badges).
