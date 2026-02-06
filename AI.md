# Repository — AI Guide

## If you are in a worktree — redirect

**MAIN_REPO_PATH:** `~/Github/project_sourceapp/sourceapp`

If your workspace root is not that path, you are in a worktree. **AI.md and the docs hierarchy are read and updated only in the main repo.** Code changes: stay in your worktree. **Coordination** lives in GitHub Project (external); derive URL from repo. For AI.md and docs: use the main repo path above.

---

Behave like a senior engineer onboarding to an existing system. Read before acting.

---

## Agent coordination (first)

**Read the informative docs first.** Then go to Project to take action.

- **Session start:** Check all worktrees for uncommitted changes; commit any before continuing. Then read [AGENT_COORDINATION_QUICK_START](docs/08-status/agent-coordination/AGENT_COORDINATION_QUICK_START.md). Follow its instructions. Then open Project → Directives view; read items; follow instructions for your role (by label).
- **Safeguard:** Only create or update Project items in the project for this workspace's repo. Derive repo from `git remote`; if unclear, ask. Do not use `gh project list` without a repo filter.
- **Assignee:** Always the active user. Use labels for role routing (`agent:backend`, `agent:web`, etc.).
- **Questions for another agent:** Create Project item with label `coordination:q-a`; add role label; assign to user. See [AGENT_COORDINATION_LOG](docs/08-status/agent-coordination/AGENT_COORDINATION_LOG.md) for format.
- **Check-out:** Optional. Create item in Active Work view; check-in = close.

**If Project does not exist yet:** Tell the user to run [GitHub Coordination Setup Guide](docs/07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) and [New Project Repo Setup SOP](docs/06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md). Until then, use docs mode per [Active Work SOP](docs/06-operations/coordination/ACTIVE_WORK_SOP.md) for check-out.

**Agent capabilities:** All agents have API access via `gh` CLI and env vars. Use them — do not defer to docs when you can act. GitHub (repos, issues, Projects), GitHub Project (items, views), Render (services, deploys), SendGrid (email) are reachable. Create Project items, run `gh` commands, call Render API when the task requires it.

---

## Adoption

**When instructed to adopt blank into another project:** Follow [Infrastructure Template Adoption SOP](docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md). Source: blank. Target: the project the user named.

---

## Before making changes

Before any code or documentation change:

1. **Daily integrity check** — If [INTEGRITY_CHECK_LOG](docs/08-status/INTEGRITY_CHECK_LOG.md) has no entry for today (YYYY-MM-DD), run [Repo Integrity Check SOP](docs/06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md) first.
2. **Check-out** (optional) — Per [Active Work SOP](docs/06-operations/coordination/ACTIVE_WORK_SOP.md): check ACTIVE_WORK; add item when starting; close when done.
3. **New behavior** — When user requests new features or significant changes: create Project item (label `coordination:feature-request`) first. Do not implement until the feature request exists. See [Documentation Process](docs/06-operations/processes/DOCUMENTATION_PROCESS.md).
4. **Read affected layer** — Layers 2–4 before coding; Layer 6 before ops.
5. **Confirm no canon violated** — [GUIDELINES](docs/02-canon/GUIDELINES.md), [PROJECT_SCOPE](docs/02-canon/PROJECT_SCOPE.md).

Full process pointers: [docs/README Process and behavior (reference)](docs/README.md#process-and-behavior-reference).

---

## Navigation

**Hub:** `[MAIN_REPO_PATH]/docs/README.md`. Choose your path — pick the layer (structural) or use the direct link (faster). Both lead to docs in the hierarchy; no orphans.

**Extended rules:** `[MAIN_REPO_PATH]/docs/02-canon/GUIDELINES.md`

---

## How to behave here

- Be surgical. Change only what is requested.
- Avoid drive-by refactors.
- Preserve authentication, session, and user-role semantics unless explicitly instructed.
- Keep frontend UX unchanged unless a change is requested.
- Use existing storage mechanisms.
- Do not introduce new storage layers without approval.
- Preserve copy and branding unless asked to change it.

If you are unsure, stop and ask.

---

## Hard rules

These override all other instructions.

- Do not implement new behavior without a feature request first. User requests for new features → create Project item (label `coordination:feature-request`) → then implement.
- Do not rename or remove public API endpoints without approval.
- Do not alter session or auth behavior unless requested.
- Do not introduce new authentication flows without approval.
- Keep database migrations minimal and idempotent.
- Never rewrite or delete existing migrations.
- Respect feature flags on both UI and API where they exist.
- **Infrastructure lock:** Only the Architect Agent may create, modify, or delete infrastructure files. See [CORE_INFRASTRUCTURE_INVENTORY](docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) for the list. Other agents: do not change these files; invoke Architect or create Project item (label `coordination:q-a`, target `agent:architect`).

---

## Objective

Your objective is not speed or cleverness.
Your objective is correctness, consistency, and safety.

Proceed accordingly.
