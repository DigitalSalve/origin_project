# Infrastructure Template Adoption — SOP

**Purpose:** Standard Operating Procedure for adopting the blank infrastructure template into an **existing** project. Use when a project already has its own structure and you want to bring in the coordination infrastructure, docs hierarchy, SOPs, and AI behavior rules without wiping the project.

**Status:** Active  
**Applies To:** Existing projects (e.g. Cheqpass, legacy projects) that need blank infrastructure  
**Related:** [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md) (for new projects), [New Project Repo Setup SOP](NEW_PROJECT_REPO_SETUP_SOP.md), [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)

---

## Overview

**Adoption** = applying blank to an existing project. Unlike bootstrap (new project from scratch), adoption requires:

1. **Copy infrastructure** from blank into the target project
2. **Merge, not replace** — preserve existing project-specific content where it exists
3. **Adapt placeholders** — MAIN_REPO_PATH, PROJECT_IDENTITY, project slug
4. **Add missing structure** — create folders/files that don't exist
5. **Set up GitHub coordination** — if not already present. If target has docs-based coordination (ACTIVE_WORK, feature-requests, reports in docs), run [Migration Plan](../../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md) after adoption.

**Use this SOP when:** An architect or agent is instructed to "update [project] infrastructure based on blank" or "adopt blank into [project]."

**Coordination model:** Read [COORDINATION_MODEL](../../01-orientation/COORDINATION_MODEL.md) for static vs dynamic (docs = read first, Project = act), what lives where, the reasoning (worktree desync), and full flow by artifact type.

---

## Prerequisites

- **Blank template path:** `~/Github/blank/` (or equivalent)
- **Target project path:** Absolute path to the existing project root (e.g. `~/Github/cheqpass/`)
- **CORE_INFRASTRUCTURE_INVENTORY.md** — Defines what to copy (read from blank)

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

**Copy from blank to target.** Use [CORE_INFRASTRUCTURE_INVENTORY](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) as the source of truth for what qualifies as infrastructure.

**For each infrastructure path:**

| Action | When |
|--------|------|
| **Copy** | File does not exist in target |
| **Merge** | File exists in both; target has project-specific content to preserve |
| **Skip** | Target has project-specific override (e.g. PROJECT_IDENTITY already filled) |

**Copy command pattern:**
```bash
BLANK=~/Github/blank
TARGET=/path/to/target-project

# Example: copy AI.md if target doesn't have it or has outdated coordination
cp "$BLANK/AI.md" "$TARGET/AI.md"
# Then merge: restore target's project-specific content if any was overwritten

# Example: copy entire coordination SOPs folder (create if missing)
mkdir -p "$TARGET/docs/06-operations/coordination"
cp "$BLANK/docs/06-operations/coordination/"*.md "$TARGET/docs/06-operations/coordination/"
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

**When target has content that blank would overwrite:**

1. **AI.md** — Blank has coordination section and feature-request-first rule. If target has AI.md:
   - Add or update the coordination section from blank
   - Ensure "Before making changes" includes: create feature request before implementing new behavior
   - Ensure "Hard rules" includes: do not implement new behavior without a feature request first
   - Preserve target's project-specific hard rules and behavior
   - Do not remove target-only sections

2. **docs/02-canon/, 03-architecture/, 04-behavior/, 05-integrations/** — **Do not overwrite.** These are project-specific. Only add missing structure (e.g. empty README) if folder is absent.

3. **docs/08-status/** — Copy folder structure and READMEs. Do not overwrite ACTIVE_WORK, reports, logs. Feature requests live in GitHub Project, not docs.

4. **PROJECT_IDENTITY** — If target has one, update only the infrastructure vs project-specific table if needed; fill name/slug/role.

---

### Step 5: GitHub Coordination Setup

**If target does not have GitHub Project "Agent Coordination":**

- [ ] Run [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)
- [ ] Create Project, link to repo, create labels, **create Project field** (Step 3b), configure views
- [ ] Add **Project** field as a column in views (Configure → enable Project field)
- [ ] Verify with checklist in the guide

**If target already has a Project:** Ensure it matches the guide (labels, views, **Project field with project slug**). Add missing labels/views/field if needed.

---

### Step 6: Verify Adoption

**Checklist:**

- [ ] [COORDINATION_MODEL](../../01-orientation/COORDINATION_MODEL.md) has Static vs Dynamic section; docs/README links to it
- [ ] AI.md has coordination section, feature-request-first rule (Before making changes + Hard rules), and correct MAIN_REPO_PATH
- [ ] docs/README.md exists and links to all layers
- [ ] docs/01-orientation/PROJECT_IDENTITY.md has correct name, slug, role
- [ ] docs/06-operations/coordination/ has AGENT_COORDINATION_SOP, ACTIVE_WORK_SOP, AGENT_SESSION_HOOK_SOP
- [ ] docs/07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md present
- [ ] .cursor/rules/read-ai-md-first.mdc present
- [ ] GitHub Project created (or already exists) with labels, **Project field** (repo identifier), and views; Project field visible as column
- [ ] No project-specific content was overwritten

---

## Agent / Architect Instructions

**When instructing an agent to adopt blank into a project, provide:**

1. **Blank path:** `~/Github/blank/` (or `@~/Github/blank/`)
2. **Target path:** Absolute path to the project (e.g. cheqpass)
3. **Explicit instruction:** "Adopt the blank infrastructure into [project]. Follow [INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP](docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md). Preserve existing project-specific content. Set MAIN_REPO_PATH and PROJECT_IDENTITY for [project]."

**Example prompt:**
> Update cheqpass's coordination infrastructure to match the blank template at `~/Github/blank/`. Follow the Infrastructure Template Adoption SOP. Copy infrastructure files, merge where target has existing content, set MAIN_REPO_PATH and PROJECT_IDENTITY for cheqpass. Do not overwrite cheqpass-specific canon, architecture, or behavior docs.

---

## Integration with Other Processes

- **CORE_INFRASTRUCTURE_INVENTORY.md** — Defines what to copy
- **BOOTSTRAP_NEW_PROJECT.md** — For new projects; adoption is for existing
- **NEW_PROJECT_REPO_SETUP_SOP.md** — MAIN_REPO_PATH and repo setup
- **GITHUB_COORDINATION_SETUP_GUIDE.md** — GitHub Project setup
- **INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md** — Project → blank (opposite direction)

---

## Related Documentation

- [Core Infrastructure Inventory](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) — What qualifies as infrastructure
- [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md) — For new projects
- [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) — Project setup
- [Infrastructure Template Sync SOP](INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md) — Sync project → blank

---

**Last Updated:** 2026-01-28
