# GitHub Coordination Migration Plan

**Purpose:** Move coordination and active-state artifacts from `docs/08-status/` to GitHub Projects and Discussions so they are consistent across worktrees and always available outside the repo. **For existing projects** that use docs-based coordination.

**Principle:** Anything actively used for coordination or collaboration (including reports created for review) must live outside the repo. Versioned snapshots (decisions, finalized docs) stay in the repo.

**New projects:** Use [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) instead. No migration needed. See [Coordination Model](../01-orientation/COORDINATION_MODEL.md) for full reasoning and flow.

---

## Current State (docs/08-status)

### Coordination / Active State (MOVE)

| Artifact | Purpose | GitHub Destination |
|----------|---------|-------------------|
| **agent-coordination/** | | |
| AGENT_DIRECTIVES | Architect → agents; session start | Project: "Directives" view or pinned Discussion |
| AGENT_COORDINATION_LOG | Agent ↔ agent Q&A; append-only | Discussions: "Agent Coordination" category |
| **ACTIVE_WORK** | Currently checked-out files | Project: "Active Work" view (items = check-outs) |
| **ACTIVE_WORK_LOG** | Check-out/check-in history | Project: activity or Discussions thread |
| **INTEGRITY_CHECK_LOG** | Daily integrity check record | Project: "Integrity Check" items or Discussion |
| **feature-requests/*.md** | FR specs (content) | Project: items with `coordination:feature-request`; spec in item body |
| **FEATURE_REQUEST_TODO** | Prioritized FR list; status | Project: Feature Requests view |
| **IMPLEMENTATION_VERIFICATION** | Open/resolved verification questions | Project: items or Issues with label |
| **QA_TODO** | QA checklist; open items | Project: QA column |
| **QA_WORK_LOG** | QA session log | Project activity or Discussion |
| **REPORT_LOG** | Master log of reports | Project: "Reports" view or Discussion |
| **screen-examination/** | Screen examination reports | Project: report items; created for review in collaboration; desyncs across worktrees |
| **backend-examination/** | Backend examination reports | Project: report items; created for review in collaboration |
| **reports/** | Session reports, architecture summaries | Project: report items; created for review in collaboration |
| **CURRENT_STATUS** | Implementation/deploy status | Project: status items or pinned Discussion |
| **SCOPE_OF_WORK** | Living SOW; deliverables | Project: SOW view or linked Discussion |

### Versioned Outputs (STAY IN REPO)

| Artifact | Purpose | Why stay |
|----------|---------|----------|
| **decisions/** | Decision log, flow decisions | Canonical; versioned |
| **user-flows/** | Flow and branching docs | Versioned behavior docs |
| **scope-of-work/** | SOW snapshots | Versioned; Project holds living status |

---

## Migration Phases

### Phase 1: Agent Coordination (highest impact)

1. Create Project views: Directives, Active Work
2. Add first directive as Project item
3. Update AI.md: "Session start: GitHub Project (Directives view)" instead of AGENT_DIRECTIVES
4. Create Discussion "Agent Coordination" for Q&A
5. Update AGENT_COORDINATION_SOP, AGENT_SESSION_HOOK_SOP to reference GitHub
6. Deprecate docs/08-status/agent-coordination/ (keep README as pointer)

### Phase 2: Active Work

1. Create "Active Work" view in Project
2. Check-out = create Project item; check-in = close/move item
3. Update ACTIVE_WORK_SOP to use `gh project item create` / `gh issue create`
4. ACTIVE_WORK_LOG → Project activity history or Discussion thread
5. Deprecate ACTIVE_WORK.md, ACTIVE_WORK_LOG.md

### Phase 3: Feature Requests & Status

1. Create one Project item per feature request; label `coordination:feature-request`
2. Put full spec in item body (Overview, Requirements, Subtasks, etc. — use FEATURE_REQUEST_TEMPLATE format)
3. Feature Requests view = all FRs; no docs/08-status/feature-requests/ for active work (desyncs across worktrees)
4. Migrate IMPLEMENTATION_VERIFICATION to Project items
4. Migrate QA_TODO, QA_WORK_LOG
5. CURRENT_STATUS → Pinned Discussion or Project "Status" view

### Phase 4: Reports & Logs

1. **Reports** → Project "Reports" view: create Project item per report; report content in item body; label `coordination:report`. Created for review in collaboration; desyncs if in docs.
2. REPORT_LOG → Project view or Discussion
3. INTEGRITY_CHECK_LOG → Discussion or Project "Integrity" items
4. SCOPE_OF_WORK living status → Project; keep scope-of-work/SCOPE_OF_WORK.md for versioned snapshots

### Phase 5: SOP & Doc Updates

1. Update all SOPs that reference moved artifacts
2. Update docs/README.md Layer 8 to point to GitHub
3. Update AI.md, GUIDELINES
4. Add Cursor rule: "Coordination lives in GitHub Project; use gh for check-out, directives, Q&A"

---

## New Project vs Migration

| Scenario | Path |
|----------|------|
| **New project** (origin_project template) | Run [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md); coordination is GitHub from start |
| **Existing project** (docs-based) | Execute migration phases above; move from docs to GitHub |

---

## Rollback

If migration causes issues: docs/08-status/ artifacts remain in repo until explicitly removed. Phase 1 can run in parallel (GitHub + docs) during transition.

---

**Created:** 2026-01-28  
**Status:** Plan; for existing projects migrating from docs
