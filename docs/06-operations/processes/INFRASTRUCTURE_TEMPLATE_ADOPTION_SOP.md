# Infrastructure Template Adoption — SOP

**Purpose:** Standard Operating Procedure for adopting the origin_project infrastructure template into an **existing** project. Use when a project already has its own structure and you want to bring in the coordination infrastructure, docs hierarchy, SOPs, and AI behavior rules without wiping the project.

**Status:** Active  
**Applies To:** Existing projects (e.g. Cheqpass, legacy projects) that need origin_project infrastructure  
**Related:** [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md) (for new projects), [New Project Repo Setup SOP](NEW_PROJECT_REPO_SETUP_SOP.md), [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)

---

## Overview

**Adoption** = applying origin_project to an existing project. Unlike bootstrap (new project from scratch), adoption requires:

1. **Copy infrastructure** from origin_project into the target project
2. **Merge, not replace** — preserve existing project-specific content where it exists
3. **Adapt placeholders** — MAIN_REPO_PATH, PROJECT_IDENTITY, project slug
4. **Add missing structure** — create folders/files that don't exist
5. **Set up GitHub coordination** — if not already present. If target has docs-based coordination (ACTIVE_WORK, feature-requests, reports in docs), update SOPs and status docs to reference Project per [Coordination Model](../../01-orientation/COORDINATION_MODEL.md).

**Use this SOP when:** An architect or agent is instructed to "update [project] infrastructure based on origin_project" or "adopt origin_project into [project]."

**Coordination model:** Read [COORDINATION_MODEL](../../01-orientation/COORDINATION_MODEL.md) for static vs dynamic (docs = read first, Project = act), what lives where, the reasoning (worktree desync), and full flow by artifact type.

---

## Prerequisites

- **origin_project template path:** `~/Github/origin_project/` (or equivalent)
- **Target project path:** Absolute path to the existing project root (e.g. `~/Github/cheqpass/`)
- **CORE_INFRASTRUCTURE_INVENTORY.md** — Defines what to copy (read from origin_project)

---

## Process

### Step 1: Assess Target Project

**Before copying, inventory what exists:**

- [ ] **Check for AI.md** — Does target have AI.md? If yes, compare coordination section; merge or replace.
- [ ] **Check for docs/** — Does target have docs hierarchy? Note which layers exist (01–08).
- [ ] **Check for .cursor/rules/** — Does target have Cursor rules?
- [ ] **Check for GitHub Project** — Is there already an "Agent Coordination" Project?

**Output:** List of what exists vs what is missing. Plan merge strategy.

---

### Step 2: Copy Infrastructure Files

**Copy from origin_project to target.** Use [CORE_INFRASTRUCTURE_INVENTORY](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) as the source of truth for what qualifies as infrastructure.

**For each infrastructure path:**

| Action | When |
|--------|------|
| **Copy** | File does not exist in target |
| **Merge** | File exists in both; target has project-specific content to preserve |
| **Skip** | Target has project-specific override (e.g. PROJECT_IDENTITY already filled) |

**Copy command pattern:**
```bash
ORIGIN_PROJECT=~/Github/origin_project
TARGET=/path/to/target-project

# Example: copy AI.md if target doesn't have it or has outdated coordination
cp "$ORIGIN_PROJECT/AI.md" "$TARGET/AI.md"
# Then merge: restore target's project-specific content if any was overwritten

# Example: copy entire coordination SOPs folder (create if missing)
mkdir -p "$TARGET/docs/06-operations/coordination"
cp "$ORIGIN_PROJECT/docs/06-operations/coordination/"*.md "$TARGET/docs/06-operations/coordination/"
```

**Key paths to copy (from CORE_INFRASTRUCTURE_INVENTORY):**

- `AI.md` — Merge coordination section; preserve project-specific hard rules
- `docs/README.md` — Copy; update layer 2–5 links if target has different structure
- `docs/01-orientation/` — Copy GETTING_STARTED, BOOTSTRAP, CORE_INFRASTRUCTURE_INVENTORY, etc.; **adapt** PROJECT_IDENTITY
- `docs/02-canon/GUIDELINES.md` — Copy (project-agnostic)
- `docs/06-operations/` — Copy all SOPs, processes, coordination, examination, conventions, management
- `docs/07-guides/` — Copy GITHUB_COORDINATION_SETUP_GUIDE, INFRASTRUCTURE_*.md, templates/
- `docs/08-status/` — Copy **structure** (folders, READMEs); do not overwrite existing contents
- `.cursor/rules/read-ai-md-first.mdc` — Copy if missing

---

### Step 3: Adapt Project-Specific Values

**Replace placeholders in the target:**

| Placeholder | Where | Action |
|-------------|-------|--------|
| **MAIN_REPO_PATH** | AI.md (top section) | Set to target project root absolute path |
| **Project name** | docs/01-orientation/PROJECT_IDENTITY.md | Set to target project name |
| **Project slug** | docs/01-orientation/PROJECT_IDENTITY.md | Set to target project slug (e.g. `cheqpass`) |
| **Repository role** | docs/01-orientation/PROJECT_IDENTITY.md | Set to target repo role (client/service/worker) |
| **Repo owner** | docs/01-orientation/PROJECT_IDENTITY.md | Set to GitHub repo owner (from `gh repo view --json owner -q .owner.login`). **Permanent** — agent coordination uses this account's Projects. |
| **Project URL** | docs/01-orientation/PROJECT_IDENTITY.md | Set to `https://github.com/[owner]/[repo]/projects` |

**Example:**
```markdown
<!-- AI.md -->
**MAIN_REPO_PATH:** ~/Github/cheqpass

<!-- PROJECT_IDENTITY -->
| **Project name** | Cheqpass |
| **Project slug** | cheqpass |
```

---

### Step 4: Merge Conflicts

**When target has content that origin_project would overwrite:**

1. **AI.md** — origin_project has coordination section and feature-request-first rule. If target has AI.md:
   - Add or update the coordination section from origin_project
   - Ensure "Before making changes" includes: create feature request before implementing new behavior
   - Ensure "Hard rules" includes: do not implement new behavior without a feature request first
   - Preserve target's project-specific hard rules and behavior
   - Do not remove target-only sections

2. **docs/02-canon/, 03-architecture/, 04-behavior/, 05-integrations/** — **Do not overwrite.** These are project-specific. Only add missing structure (e.g. empty README) if folder is absent.

3. **docs/08-status/** — Copy folder structure and READMEs. Do not overwrite ACTIVE_WORK, reports, logs. Feature requests live in GitHub Project, not docs.

4. **PROJECT_IDENTITY** — If target has one, update only the infrastructure vs project-specific table if needed; fill name/slug/role.

---

### Step 5: GitHub Coordination Setup (establish owner permanently)

**Establish repo owner's Project during adoption.** This designation is **permanent** — recorded in PROJECT_IDENTITY; agents read it at session start.

#### 5.1: Derive repo owner and check permissions

```bash
cd /path/to/target-project
# Get repo owner
OWNER=$(gh repo view --json owner -q .owner.login)
CURRENT_USER=$(gh api user --jq .login)

echo "Repo owner: $OWNER"
echo "Current user: $CURRENT_USER"
```

**Permission check:**

- **If current user = repo owner:** Proceed with `--owner @me`
- **If current user ≠ repo owner:** Test if you can create under owner:
  ```bash
  gh project list --owner "$OWNER" || echo "Cannot access $OWNER's projects"
  ```
  - **If accessible:** Try `gh project create --owner "$OWNER"` (may require org membership)
  - **If not accessible or creation fails:** See [Permission fallback](#permission-fallback-when-current-user-≠-owner) below

#### 5.2: Create and link Project

**If you have permission:**

```bash
# Create (use $OWNER if accessible; @me if owner == current user)
gh project create --owner "$OWNER" --title "Agent Coordination"
# Returns: project number (record it)

# Link to repo
gh project link [n] --repo "$OWNER/[repo]"
```

**Record the project number** for Step 5.4.

#### 5.3: Create labels, field, and views

Per [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md):
- Create 21 labels
- Create Project field (single-select with project slug)
- Create 8 views (Directives, Active Work, Feature Requests, etc.)

#### 5.4: Record in PROJECT_IDENTITY

Add to `docs/01-orientation/PROJECT_IDENTITY.md` (GitHub coordination section):

| Field | Value |
|-------|-------|
| **Repo owner** | [owner from Step 5.1] |
| **Project URL** | `https://github.com/[owner]/[repo]/projects` |

**Permanent.** Do not change after adoption.

#### Permission fallback (when current user ≠ owner)

**If you cannot create projects under the repo owner:**

1. **Ask the repo owner to:**
   - Create "Agent Coordination" project: `gh project create --owner @me --title "Agent Coordination"`
   - Link to repo: `gh project link [n] --repo [owner/repo]`
   - Run Steps 5.3 (labels, field, views)
   - Grant you access: Project → Settings → Manage access → Add collaborator
   - Share project number/URL

2. **Alternative (less ideal):** Create under your account with `--owner @me`. Record in PROJECT_IDENTITY that Project is under your account, not the repo owner. Notify when handing off to others.

3. **Record in PROJECT_IDENTITY:** Note who owns the Project. If different from repo owner, add comment: "Project under [user], not repo owner [owner]."

**If target already has a Project:** Verify owner, link, labels, views; record repo owner and Project URL in PROJECT_IDENTITY.

---

### Step 6: Verify Adoption

**Checklist:**

- [ ] [COORDINATION_MODEL](../../01-orientation/COORDINATION_MODEL.md) has Static vs Dynamic section; docs/README links to it
- [ ] AI.md has coordination section, feature-request-first rule (Before making changes + Hard rules), and correct MAIN_REPO_PATH
- [ ] docs/README.md exists and links to all layers
- [ ] docs/01-orientation/PROJECT_IDENTITY.md has correct name, slug, role, **repo owner**, and Project URL
- [ ] docs/06-operations/coordination/ has AGENT_COORDINATION_SOP, ACTIVE_WORK_SOP, AGENT_SESSION_HOOK_SOP
- [ ] docs/07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md present
- [ ] .cursor/rules/read-ai-md-first.mdc present
- [ ] GitHub Project created (or already exists) with labels, **Project field** (repo identifier), and views; Project field visible as column
- [ ] No project-specific content was overwritten

---

## Agent / Architect Instructions

**When instructing an agent to adopt origin_project into a project, provide:**

1. **origin_project path:** `~/Github/origin_project/`
2. **Target path:** Absolute path to the project (e.g. cheqpass)
3. **Explicit instruction:** "Adopt the origin_project infrastructure into [project]. Follow [INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP](docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md). Preserve existing project-specific content. Set MAIN_REPO_PATH and PROJECT_IDENTITY for [project]."

**Example prompt:**
> Update cheqpass's coordination infrastructure to match the origin_project template at `~/Github/origin_project/`. Follow the Infrastructure Template Adoption SOP. Copy infrastructure files, merge where target has existing content, set MAIN_REPO_PATH and PROJECT_IDENTITY for cheqpass. Do not overwrite cheqpass-specific canon, architecture, or behavior docs.

---

## Integration with Other Processes

- **CORE_INFRASTRUCTURE_INVENTORY.md** — Defines what to copy
- **BOOTSTRAP_NEW_PROJECT.md** — For new projects; adoption is for existing
- **NEW_PROJECT_REPO_SETUP_SOP.md** — MAIN_REPO_PATH and repo setup
- **GITHUB_COORDINATION_SETUP_GUIDE.md** — GitHub Project setup
- **INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md** — Project → origin_project (opposite direction)

---

## Related Documentation

- [Core Infrastructure Inventory](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) — What qualifies as infrastructure
- [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md) — For new projects
- [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) — Project setup
- [Infrastructure Template Sync SOP](INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md) — Sync project → origin_project

---

**Last Updated:** 2026-01-28
