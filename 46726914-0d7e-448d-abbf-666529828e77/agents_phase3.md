# Phase 3 – Personal & Task Agents (Work Log)

## Scope
- Personal agents per user: created automatically (or via UI) with scoped knowledge/permissions.
- Task agents (templates) for common workflows (file analyzer, policy generator, summarizer, threat intel, etc.).
- Enforce scope checks on agent runs (tenant/user/role/knowledge_scope); no cross-tenant leakage.
- Seed: ensure admin has personal agent after reset (reuse seed script).

## Plan
1) Templates/types:
   - Define allowed `type` values and optional template presets for task agents (provider/prompt defaults).
2) Scoping enforcement:
   - In AgentRunner, validate agent type/owner and restrict accessible artifacts/evidence/policies by tenant and knowledge_scope.
   - Reject runs if caller lacks access or agent type mismatch (e.g., personal without owner).
3) Personal agent creation flows:
   - Backend endpoint to auto-create personal agent for a user (already added).
   - UI trigger (future phase) to create personal agent from profile.
4) Task agents:
   - Add creation helper (optional endpoint) to instantiate template agents with pre-set prompts/permissions.
   - Templates can be collected from the first client’s needs (e.g., file analyzer, policy generator, summarizer, threat intel). Make them configurable via env/JSON for easy iteration.
5) Seeds:
   - Update/verify seed (scripts/seed_admin_and_agents.win.ps1) to keep admin personal agent; add sample task agent if desired.
6) Tests:
   - Backend: scoping checks on run (tenant/knowledge_scope), personal agent ownership, and task agent creation.
   - (Frontend in later phase) smoke for create personal/task agent once UI exists.

## Execution Log
- [x] Scope checks enforced in AgentRunner (knowledge_scope filtering; personal agent requires owner)
- [x] Seed verified (admin personal agent present via scripts/seed_admin_and_agents.win.ps1)
- [x] Task agent templates + helper endpoint (/v1/ai/agent-tasks/{template_key})
- [ ] Personal agent creation flow verified (manual test)
- [ ] Backend tests run (manual/automated) for scoping/personal/task
- [ ] Issues encountered: (to fill)
- [ ] Status/outcome: (to fill)

## Issues & Notes (to fill during execution)
- …
