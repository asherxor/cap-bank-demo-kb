# Phase 4 – Dual Storage & Sync Automation (Work Log)

## Scope
- Dual storage for knowledge/evidence: local (offline-first) with sync to GitHub (manual + auto on critical events).
- Track sync status (pending/synced/failed) and local paths in metadata.
- Manual sync API; auto-sync hooks for critical changes; audit log for sync operations.
- Seed: ensure admin/tenant/plan/agents remain after reset; mark initial items as synced/pending appropriately.

## Plan
1) Metadata:
   - Use `meta_json` to store `sync_status`, `sync_last_synced_at`, `local_path` for artifacts/evidence/snapshots.
   - Mark new items as `pending`.
2) Sync API:
   - `/v1/knowledge/sync` implements GitHub push (token/owner/repo/ref from env); batch commit pending items; set status to synced/failed with reason + audit entry.
3) Automation:
   - Hooks for critical events (policy created, evidence flagged high sensitivity, new snapshot) → enqueue auto-sync (best-effort; if offline, remain pending).
4) Audit:
   - Log sync attempts/results (table or reuse audit_log) with counts and failures.
5) Seed:
   - Seed scripts keep admin/tenant/plan/agents; any seeded artifacts/evidence should be marked `synced` to avoid re-pushing.
6) Tests:
   - Backend: status transitions pending→synced/failed; GitHub push mock; hook marks pending.
   - Frontend (later phases): show sync status and manual “sync now” action.

## Execution Log
- [x] Metadata fields tracked (evidence + artifacts set sync_status/local_path; index marked stale)
- [x] Sync API implemented with GitHub push + status transitions ( /v1/knowledge/sync uses GitHub token/owner/repo)
- [x] Auto-sync hooks for evidence and artifacts (mark stale/pending)
- [x] Audit of sync operations implemented
- [x] Seed verified (seed seeds tenant/admin/plan/agents only; no artifacts/evidence to sync yet)
- [ ] Tests run (manual/automated) for sync transitions/hooks
- [ ] Issues encountered: (to fill)
- [ ] Status/outcome: Partial (audit added; sync API/hooks live; pending tests)

## Issues & Notes (to fill during execution)
- GitHub sync requires env vars: GITHUB_ANCHOR_TOKEN/OWNER/REPO. Missing values return 400.
- Need to verify seeded data marks sync_status correctly (currently seeds only tenant/admin/plan/agents).
- Tests pending for sync status transitions and hooks; semantic search still placeholder.
- Snapshot creation now marks artifact sync_status pending + local_path and flags index stale; consider per-snapshot sync metadata if needed later.
