# Phase 1 – Data Model & API Foundations (Work Log)

## Scope
- Extend `ai_agents` with type/owner/scope/permissions fields.
- Add Agents CRUD + personal-agent creation endpoint.
- Add sync metadata support for knowledge/evidence (pending/synced/failed), and `/v1/knowledge/sync` placeholder.
- Seed essentials after reset: tenant, admin user, plan (enterprise), supervisor agent (and optional personal agent for admin).

## Plan
1) Migration: add fields to `ai_agents` (type, owner_user_id, owner_role_id, parent_agent_id, knowledge_scope JSON, permissions JSON).
2) Schemas/services/routes: CRUD with new fields; endpoint to create personal agent.
3) Sync metadata: leverage `meta_json` or add columns? (aim: minimal schema change; meta_json per artifact/evidence).
4) Endpoint `/v1/knowledge/sync` (placeholder: mark pending → synced/failed).
5) Seed: tenant + admin@cap.local + super_admin role + supervisor agent (type=supervisor).
6) Tests (backend) for CRUD, personal agent creation, sync status transition.

## Execution Log
- [x] Migration applied (0011_agents_ext)
- [x] Backend routes updated (create/list/run + added patch/delete + personal agent; payloads include type/owner/parent/scope/permissions)
- [x] Sync endpoint in place (placeholder /v1/knowledge/sync marks pending→synced)
- [x] Seed updated (scripts/seed_admin_and_agents.win.ps1 seeds tenant/admin/plan + supervisor/personal agent)
- [x] Backend tests (manual) run: smoke CRUD (create/list/delete) + personal agent + sync endpoint (marks items as synced)
- [ ] Issues encountered: (to fill)
- [ ] Status/outcome: (to fill)

## Issues & Notes (to fill during execution)
- Smoke tests passed; no automated suite added yet. Fill in if further issues arise.
