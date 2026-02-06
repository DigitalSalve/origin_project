# GitHub Coordination Setup Guide

**Purpose:** Complete guide for setting up GitHub Projects and Discussions as the **agent coordination layer** when starting a new project. Use this when bootstrapping from the origin_project template so agents have a unified, worktree-consistent coordination point from day one.

**Status:** Infrastructure (reusable)  
**Applies to:** New projects using the origin_project template  
**Related:** [Coordination Model](../01-orientation/COORDINATION_MODEL.md) (what lives where and why), [Bootstrap New Project](../01-orientation/BOOTSTRAP_NEW_PROJECT.md), [Migration Plan](../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md) (for existing projects)

---

## Why GitHub Coordination

| Problem | Solution |
|--------|----------|
| Coordination files in `docs/08-status/` diverge across worktrees | GitHub Project and Discussions live outside the repo; same view everywhere |
| Agents in different branches see different AGENT_DIRECTIVES | Single source of truth in GitHub |
| Manual sync of coordination between main and worktrees | No sync needed; GitHub is external |
| New projects must re-derive coordination logistics | This guide + origin_project template = ready-to-use setup |

**Principle:** Anything actively used for coordination or collaboration lives outside the repo. Feature requests and reports (created for review) live in the Project — they desync across worktrees if in docs. Versioned snapshots (decisions, finalized docs) stay in the repo.

---

## Project URL Discovery

**Derive from repo:** The Project URL is `https://github.com/{owner}/{repo}/projects`. Agents infer the repo from the workspace (e.g. `git remote`, workspace path). If the repo is unclear, **ask the user**.

No hardcoded URL needed. After creating the Project, **link it to the repo** so it appears in the repo's Projects tab.

**Project number for API/CLI:** When using `gh project item-create` or the GraphQL API, you need the project number. It appears in the URL when viewing the project: `https://github.com/{owner}/{repo}/projects/1` → project number is `1`. Use `gh project list --owner @me` to list projects; the number is the first column. For repo-scoped access, open the repo's Projects tab and use the number from that project's URL.

---

## Assignee and Labels

**Assignee:** Always the **GitHub user** (the active user by default). Agents never assign to "agent." When you work with an agent, items are assigned to you unless you specify another user with repo access.

**When creating issues via `gh issue create`:** Always add `--assignee @me` (or `--assignee username`) so the item is assigned.

**Role routing:** Use **labels** so agents know their scope. Agents read labels to determine which items to respond to. Create these labels in the repo:

| Label | Purpose |
|-------|---------|
| `agent:backend` | Backend agent scope |
| `agent:web` | Web agent scope |
| `agent:webapp` | Webapp agent scope |
| `agent:mobile` | Mobile agent scope |
| `agent:database` | Database agent scope |
| `agent:storage` | Storage agent scope |
| `agent:architect` | Architect agent scope |
| `agent:documentation` | Documentation agent scope |
| `agent:qa` | QA agent scope |
| `coordination:directive` | Architect directive |
| `coordination:q-a` | Agent Q&A |
| `coordination:active-work` | Active work / check-out |
| `coordination:feature-request` | Feature request item |
| `coordination:report` | Report item (created for review in collaboration) |
| `coordination:implementation-verification` | Implementation verification question |
| `coordination:qa` | QA item |
| `coordination:integrity-check` | Integrity check record |
| `coordination:next-steps` | Next steps / execution plan item |
| `status:open` | Open |
| `status:resolved` | Resolved |

Tasks may have multiple labels (e.g. `agent:backend` + `agent:database`).

---

## Prerequisites

### 1. GitHub CLI (`gh`)

```bash
brew install gh
gh --version
```

### 2. Repository

- Project repository exists on GitHub
- You have admin or write access

### 3. Authentication

**Fine-grained personal access token:**

1. GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Repository access: Only select repositories → choose this project's repo
3. Permissions: **Projects** (Read and write), **Contents** (Read and write), **Metadata** (Read-only)
4. Authenticate: `gh auth login` (paste token)

---

## Setup Steps

### Step 1: Create GitHub Project

```bash
cd /path/to/your-project
gh project create --owner @me --title "Agent Coordination"
```

Or via UI: Repo → Projects → New project.

### Step 2: Link Project to Repo

```bash
gh project link <project-number> --repo owner/repo
```

Example: `gh project link 1 --repo dustintaylored/sourceapp`

The Project then appears at `https://github.com/owner/repo/projects`. Agents derive this URL from the repo.

### Step 3: Create Labels

Create all labels in the Label List above (21 labels including `coordination:report`). Via UI: Repo → Issues → Labels → New label. Or via API/script. Each view (Step 4) requires its label to exist.

### Step 3b: Create Project field (repo identifier)

Add a **Project** field so items are identifiable by repo. Use the project slug from PROJECT_IDENTITY (e.g. `sourceapp`).

```bash
gh project field-create <project-number> --owner @me --name "Project" --data-type SINGLE_SELECT --single-select-options "<project-slug>"
```

Example: `gh project field-create 1 --owner @me --name "Project" --data-type SINGLE_SELECT --single-select-options "sourceapp"`

**Note:** `Repo` is a reserved name; use `Project` instead. When creating items, set this field to the project slug so humans can filter and identify items across multiple coordination projects.

**Add as column:** In each view, click the `+` (Add column) or the view menu → **Configure** → enable the **Project** field so it displays as a column. This shows the repo/project name on each item.

**Set on existing items:** If items were created before the field existed, use `gh project item-edit` to set Project = project slug on each item (or run a script that does this).

### Step 4: Configure Project Views

Create a view for each row. In GitHub Projects: add view → set filter by label.

| View | Filter (label) | Purpose |
|------|---------------|---------|
| **Directives** | `coordination:directive` | Architect → agents; read at session start |
| **Active Work** | `coordination:active-work` | Current check-outs (optional; worktrees provide isolation) |
| **Feature Requests** | `coordination:feature-request` | FR = Project item; full spec in item body (Overview, Requirements, Subtasks) |
| **Reports** | `coordination:report` | Reports created for review in collaboration; content in item body |
| **Implementation Verification** | `coordination:implementation-verification` | Open questions |
| **QA** | `coordination:qa` | QA items |
| **Integrity Check** | `coordination:integrity-check` | Daily integrity record |
| **Next Steps** | `coordination:next-steps` | Execution plan items |

**How to create a view:** Project → Views → New view → Name it (e.g. "Directives") → Add filter: Label = `coordination:directive`.

### Step 5: Update AI.md

Replace the "Agent coordination" section with:

```markdown
## Agent coordination (first)

Coordination lives in GitHub Project. Derive URL from repo: https://github.com/{owner}/{repo}/projects. If repo unclear, ask the user.

- **Session start:** Open Project → Directives view. Read items. Follow instructions for your role (by label).
- **Assignee:** Always the active user. Use labels for role routing (agent:backend, etc.).
- **Questions for another agent:** Create Project item with label `coordination:q-a`; assign to user.
- **Check-out:** Optional. Create item in Active Work view; check-in = close.
```

### Step 6: Deprecate docs/08-status/agent-coordination/

Keep a minimal README: "Coordination lives in GitHub. See [Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)."

---

## Projects → Repo Docs Flow

When work in Projects or Discussions becomes **stable, canonical, or decided**, promote it to versioned docs in the repo:

| Source | Promote to |
|--------|------------|
| Directive → architecture decision | `docs/03-architecture/` or Decision Log |
| Q&A → canon update | `docs/02-canon/` |
| Q&A → behavior clarification | `docs/04-behavior/` |
| Q&A → feature spec | Create Project item with `coordination:feature-request`; spec in item body |
| Coordination report | `docs/08-status/agent-coordination/YYYY-MM-DD_TOPIC.md` |

**Process:** Create or update the repo doc; add a comment to the Project item with `**Promoted to:** [doc path]`. The repo doc is versioned; the Project item remains the record.

---

## Agent Workflow (Post-Setup)

### Session start

1. Derive Project URL from repo (`/projects`); ask user if unclear
2. Verify the project is for this workspace's repo. Do not use `gh project list` without a repo filter
3. Open Directives view; read items. Filter by **Project** field = project slug when applicable
4. Check items with labels matching your role (e.g. `agent:backend`). Filter by **Project** field when applicable
5. Respond to open items; set Status to Resolved

### Create directive (Architect)

- Create Project item in Directives view
- Label: `coordination:directive`, plus role labels (e.g. `agent:backend`)
- Set **Project** field to project slug (e.g. `sourceapp`)
- Assign to user(s) as needed

### Question for another agent

- Create Project item with label `coordination:q-a`
- Add role label for target scope (e.g. `agent:architect`)
- Set **Project** field to project slug
- Assign to user (they will route to the right agent)

### Check-out (optional)

- Create item in Active Work view; label `coordination:active-work`
- Set **Project** field to project slug
- **Assign to user:** `--assignee @me`
- Check-in = close item

### Create feature request

- Create issue: `gh issue create --title "FR: Name" --body "..." --label "coordination:feature-request" --assignee @me`
- Add to project; set **Project** field to project slug; add role labels (e.g. `agent:backend`)
- See [Documentation Process](../06-operations/processes/DOCUMENTATION_PROCESS.md) for format and completion process

### Create report (for review in collaboration)

- Create issue: `gh issue create --title "Report: Name" --body "..." --label "coordination:report" --assignee @me`
- Add to project; set **Project** field; assign to reviewer(s) if different from creator
- Reports are created for review; live in Project to avoid desync across worktrees

---

## Verification Checklist

- [ ] `gh auth status` shows authenticated
- [ ] MAIN_REPO_PATH set in AI.md (see [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md))
- [ ] Project created and linked to repo
- [ ] All 20 labels created
- [ ] **Project** field created with project slug as option
- [ ] All 8 views configured (Directives, Active Work, Feature Requests, Reports, Implementation Verification, QA, Integrity Check, Next Steps)
- [ ] AI.md coordination section present (origin_project template has it)
- [ ] Project URL derivable from repo

---

## Troubleshooting

**gh project: permission denied** — Use fine-grained token with Projects: Read and write.

**Project not visible** — Ensure Project is linked to repo: `gh project link <n> --repo owner/repo`.

**Repo unclear** — Ask the user which repo this project uses.

---

## Related

- [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md) — Repo creation and MAIN_REPO_PATH
- [Bootstrap New Project](../01-orientation/BOOTSTRAP_NEW_PROJECT.md)
- [Migration Plan](../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md)
- [Core Infrastructure Inventory](../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md)

---

**Created:** 2026-01-28  
**Updated:** 2026-01-28 — Assignee=user, labels, derive URL, Projects→repo flow  
**Updated:** 2026-01-28 — Project field: add as column, set on items; explicit --assignee @me in workflows
