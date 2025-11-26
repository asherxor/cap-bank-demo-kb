# CAP Agents & Knowledge Base Orchestration Plan

## Objectives
- Introduce a structured agent ecosystem (Supervisor, Personal, Specialized) aligned with CAP permissions and knowledge scopes.
- Enable dual storage for knowledge artifacts/evidence (local + GitHub sync) with offline-first behavior.
- Add semantic search/indexing managed by a Supervisor agent, scoped per tenant/user.
- Provide UI and API to create, manage, and run agents; trigger automated tasks on key events.

## Current State (as observed)
- Tables `ai_agents` / `ai_agent_runs` exist; no type/owner/scope fields; no UI for agents.
- Knowledge base models exist: `knowledge_artifacts`, `knowledge_artifact_snapshots`, `knowledge_links`; no semantic index; no sync metadata.
- Evidence upload supports single/multi-file; minimal metadata; no GitHub sync or semantic ingest.
- No Supervisor/Personal agents; runs are generic; no RLS-based scoping for agents.

## Phased Plan (with validation after each phase)

### Phase 1 – Data Model & API Foundations (High Priority)
- Extend `ai_agents`:
  - Fields: `type` (supervisor/personal/task), `owner_user_id`, `owner_role_id`, `parent_agent_id`, `knowledge_scope` (JSON list of artifact_ids/tags), `permissions` (JSON).
  - Migrations + minimal validation (enforce tenant + owner presence for personal).
- Agents API:
  - CRUD with new fields.
  - Endpoint to create a personal agent for a given user (auto-scope based on role/tenant).
  - Run endpoint injects user/tenant scope into prompt.
- Knowledge/Evidence sync metadata:
  - Add `sync_status` (pending/synced/failed) and `local_path` in metadata for artifacts/evidence/snapshots.
  - Event hook after create/upload → mark pending_sync.
- New endpoint `/v1/knowledge/sync`:
  - Push pending items to GitHub (using env token/owner/repo/ref); commit batching; mark status.
  - Safe mode: skip unauthorized or missing token.
- **Validation**:
  - Backend: migration + unit/API tests for CRUD agents, create personal agent, sync metadata fields, `/knowledge/sync` happy-path and failure modes (missing token).
  - Frontend (if impacted): smoke check apiClient changes (no UI yet).

### Phase 2 – Supervisor Agent & Semantic Index (High Priority)
- Create a Supervisor agent (one per tenant or global flag):
  - Permissions: full read/write on KB; approve/dismiss other agent outputs.
  - Tasks:
    - Build semantic index from snapshots (content_text/summary_text) and evidence text when available.
    - Store index metadata (version, last_built_at) in a lightweight table or `meta_json`.
  - Endpoint `/v1/agents/supervisor/reindex`.
- Search API:
  - `/v1/search/semantic` → query index scoped by tenant/user/role and `knowledge_scope`.
- Background/trigger:
  - On new artifact/snapshot/evidence → enqueue “index update” job for Supervisor (best-effort sync/offline-friendly).
- **Validation**:
  - Backend: API tests for Supervisor creation, `/reindex`, and `/search/semantic` with scoped results; index metadata stored.
  - Frontend: if search bar added in this phase, validate search results and permissions scoping.

### Phase 3 – Personal & Task Agents (Medium Priority)
- Personal Agents:
  - Auto-create on first login or via profile button.
  - Scope: tenant + role-based KB subset; limited write (e.g., drafts).
  - Runs include user context and allowed sources.
- Task Agents:
  - Templates: File Analyzer, Policy Generator, Summarizer, Threat Intel, etc.
  - Optional parent = Supervisor; outputs sent for Supervisor review (approve/reject) via `ai_agent_run` promotion metadata.
- RLS/Authorization:
  - Enforce scope checks in agent run: only allowed artifacts/evidence; block cross-tenant access.
- **Validation**:
  - Backend: tests for personal agent creation tied to user/role, run scoping, and forbidden access across tenants.
  - Frontend: UI tests (manual or automated) for create personal agent, run, and view history.

### Phase 4 – Dual Storage & Sync Automation (Medium Priority)
- Local storage layout: `data/knowledge/<tenant>/<artifact_type>/<sha>/...`.
- Sync triggers:
  - Manual: `/v1/knowledge/sync`.
  - Auto: critical events (policy created, evidence marked high sensitivity) → attempt push; if offline, remain pending.
- Audit log for sync operations (success/failure, items pushed).
- **Validation**:
  - Backend: tests for sync status transitions (pending → synced/failed), GitHub push mock tests.
  - Frontend: display sync status, manual “sync now” action works.

### Phase 5 – Frontend UX (Medium Priority)
- New “Agents” screen:
  - List/create/edit/delete agents (type, owner, scope, permissions).
  - Run agent: prompt + select scope/sources; show run history/results.
  - Buttons: “Create Personal Agent” (from profile), “Reindex” (Supervisor), “Sync KB” (manual).
- Evidence screen:
  - Show sync status and analysis metadata; optional “Send to GitHub now” action.
- Semantic search bar:
  - Call `/v1/search/semantic` with current user/tenant scope.
- **Validation**:
  - Frontend: component-level and e2e smoke for Agents screen, run flow, sync status display, semantic search results.
  - Backend: confirm UI calls map to tested endpoints.

### Phase 6 – Automation & Quality Gates (Lower Priority)
- Event hooks: on Evidence upload/Policy creation → invoke appropriate task agent; queue run; notify Supervisor for approval.
- Promotion workflow: Supervisor can approve/attach outputs as artifacts/evidence; log decisions.
- Health checks: index freshness, pending sync count, failed runs.
- **Validation**:
  - Backend: tests for event hooks triggering runs, promotion approval flow, health endpoints.
  - Frontend: notification/UX checks if surfaced; manual sanity if automated not feasible.

## Risks & Mitigations
- GitHub token/sync failures: keep pending_sync state, allow manual retry; do not block local availability.
- Security: enforce scope checks on every agent run; avoid cross-tenant leakage.
- Index size/performance: start with lightweight local index; consider pluggable vector store later.

## Deliverables (Incremental)
- Phase 1: migrations + agents CRUD + sync metadata + /knowledge/sync endpoint.
- Phase 2: Supervisor agent + semantic index build/search + reindex endpoint.
- Phase 3: Personal/task agents with scoped runs + templates.
- Phase 4: Dual storage with pending/synced/failed tracking + sync pushes to GitHub.
- Phase 5: Frontend Agents UI + sync/status indicators + semantic search.
- Phase 6: Automation hooks + promotion/approval flow + health metrics.
