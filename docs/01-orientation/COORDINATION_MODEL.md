# Coordination Model — What Lives Where and Why

**Purpose:** Single source of truth for the coordination model: what lives in GitHub Project vs docs, the reasoning, and the full flow. Use when onboarding, adopting origin_project, or deciding where to put coordination artifacts.

**Status:** Infrastructure (reusable)  
**Related:** [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md), [Migration Plan](../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md)

---

## The Problem: Worktree Desync

When using **worktrees** (multiple checkouts of the same repo for parallel work):

- Each worktree has its own copy of `docs/`
- Coordination files in `docs/08-status/` (ACTIVE_WORK, feature-requests, reports, logs) **diverge** across worktrees
- Agent in worktree A sees different state than agent in worktree B
- Manual sync is error-prone; merge conflicts on append-only logs

**Solution:** Anything used for **coordination or collaboration** lives **outside the repo** (GitHub Project, Discussions). Same view everywhere.

---

## Static vs Dynamic — The Rule

| Type | Where | Purpose |
|------|-------|---------|
| **Static** (informative, versioned) | Repo (`docs/`) | Read first. Process, conventions, specs. Tells you what to do and how. |
| **Dynamic** (interactive, live) | GitHub Project | Act here. Directives, Q&A, check-outs, feature requests. |

**Flow:** Read static docs → go to Project to take action.

---

## The Principle

| Category | Lives in | Why |
|----------|----------|-----|
| **Coordination** — directives, Q&A, check-outs | GitHub Project | Desyncs across worktrees; single source of truth |
| **Collaboration** — feature requests, reports (for review) | GitHub Project | Created for review; desyncs during collaboration |
| **Append-only logs** — REPORT_LOG, INTEGRITY_CHECK_LOG, ACTIVE_WORK_LOG | GitHub Project or Discussion | Desyncs when multiple worktrees append |
| **Versioned outputs** — decisions, finalized docs | Repo (`docs/`) | Canonical; versioned with code; no live collaboration |

**Rule:** If it's **live state** or **created for collaboration**, it goes to Project. If it's **finalized and canonical**, it stays in the repo.

---

## What Lives Where (Full Map)

### In GitHub Project (labels and views)

| Artifact | Label | View | Flow |
|----------|-------|------|------|
| **Directives** | `coordination:directive` | Directives | Architect creates → agents read at session start |
| **Q&A** | `coordination:q-a` | (filter by label) | Agent creates → assign to user → response |
| **Active Work** | `coordination:active-work` | Active Work | Check-out = create item; check-in = close |
| **Feature Requests** | `coordination:feature-request` | Feature Requests | Create → implement (subtasks) → close when complete |
| **Reports** | `coordination:report` | Reports | Create for review → assign reviewer → review → optionally promote to docs |
| **Implementation Verification** | `coordination:implementation-verification` | Implementation Verification | Open question → verify → resolve |
| **QA** | `coordination:qa` | QA | QA items and checklist |
| **Integrity Check** | `coordination:integrity-check` | Integrity Check | Daily record |
| **Next Steps** | `coordination:next-steps` | Next Steps | Execution plan items |

### In Repo (docs/)

| Artifact | Location | Why |
|----------|----------|-----|
| **Decisions** | `docs/08-status/decisions/` | Canonical; versioned; immutable once decided |
| **User flows** | `docs/04-behavior/`, `docs/08-status/user-flows/` | Versioned behavior docs |
| **SOW snapshots** | `docs/08-status/scope-of-work/` | Versioned; Project holds living status |
| **Promoted reports** | Optional: `docs/08-status/reports/` | After review, optional versioned snapshot |

---

## Full Flow by Artifact Type

### Feature Request
1. **Create** — Project item, label `coordination:feature-request`, spec in body
2. **Implement** — Mark subtasks in body; note deviations
3. **Complete** — All subtasks done → update body with completion notes → close item
4. **Optional** — Create feature note in `docs/07-guides/` for versioned record

### Report (for review)
1. **Create** — Project item, label `coordination:report`, content in body
2. **Assign** — Assign to reviewer(s)
3. **Review** — Reviewer comments; author revises
4. **Complete** — Close when review done
5. **Optional** — Promote to `docs/08-status/reports/` or examination folder for versioned snapshot

### Directive
1. **Create** — Architect creates Project item, label `coordination:directive`
2. **Read** — Agents read Directives view at session start
3. **Act** — Follow instructions; close or update as needed

### Active Work (check-out)
1. **Check-out** — Create Project item, label `coordination:active-work`; or use docs if not yet migrated
2. **Work** — Edit files in worktree
3. **Check-in** — Close item; or append to ACTIVE_WORK_LOG if using docs

---

## Migration State

**Implemented (SOPs updated):** Feature requests → Project

**Planned (see Migration Plan):** Directives, Active Work, Reports, Implementation Verification, QA, Integrity Check, REPORT_LOG, CURRENT_STATUS, SCOPE_OF_WORK

**New projects:** Run [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md); coordination is Project from day one.

**Existing projects with docs-based coordination:** Run [Migration Plan](../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md) after adopting origin_project.

---

## Adoption

When **adopting origin_project into an existing project**:

1. Copy infrastructure per [Adoption SOP](../06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md)
2. Set up GitHub Project per [Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) if not present
3. **If target has docs-based coordination** (ACTIVE_WORK, feature-requests in docs, etc.): Run [Migration Plan](../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md) to migrate those artifacts to Project

---

**Last Updated:** 2026-01-28
