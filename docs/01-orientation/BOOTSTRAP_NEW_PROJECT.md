# Bootstrap New Project from This Infrastructure

**Purpose:** How to take this **core infrastructure** to a new project **without wiping the source**. You reuse the same infrastructure every time; only project-specific content is replaced or filled in the **new** project. The original repo stays intact.

**Use when:** Starting a new project that will use this documentation structure. You want a clean way to spawn a new project from the infrastructure you’re building here.

---

## Core idea

- **Infrastructure** = files and folders that are reusable as-is (AI.md, docs/README, 06-operations, 07-guides/templates, 08-status *structure*). You **never wipe** these; you **copy** them into a new project.
- **Project-specific** = files and folders that **exist in every project** but have **different content** per project (PROJECT_IDENTITY, 02-canon, 03-architecture, 04-behavior, 05-integrations, legal/content contents, 08-status *contents*). In a new project, you **replace** or **fill** these with that project’s data; the infrastructure copy gives you the right **placeholders or empty structure** so you don’t have to recreate layout from scratch.

You do **not** wipe this repo. You either (1) **copy this repo** to a new location and then replace only project-specific parts in the copy, or (2) **clone a separate “template” repo** that holds only infrastructure + placeholders, and fill project-specific parts there.

---

## Workflow 1: Copy this repo to a new project

Use when this repo is your single source of truth for both infrastructure and one project (e.g. SourceApp). To start “Project B,” you **copy the whole repo** to a new directory or new repo, then in the **copy only** you replace project-specific content.

### Steps

1. **Copy (do not move) this repo** to a new location:
   - **Option A:** `git clone <this-repo-url> <new-project-name>` then `cd <new-project-name>` and optionally `git remote set-url origin <new-repo-url>` if Project B has its own repo.
   - **Option B:** Copy the folder (e.g. `cp -r sourceapp ../acme-docs`). If using Git, `cd ../acme-docs` and `git remote set-url origin <new-repo-url>` or init a new repo.

2. **In the new copy only**, set MAIN_REPO_PATH and replace project-specific content:
   - **MAIN_REPO_PATH:** Set in `AI.md` (absolute path to project root). See [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md).
   - **PROJECT_IDENTITY:** Overwrite `docs/01-orientation/PROJECT_IDENTITY.md` with the new project’s name, slug, repository role, and infrastructure vs project-specific section (you can keep the section structure and change only the tables).
   - **Layers 2–5:** Replace or clear `docs/02-canon/`, `docs/03-architecture/`, `docs/04-behavior/`, `docs/05-integrations/` with the new project’s canon, architecture, behavior, and integrations. Delete SourceApp-specific files; add or overwrite with Project B’s content. Standard file names (e.g. SYSTEM_MAP.md, AUTHENTICATION_FLOWS.md) are in `docs/README.md` and `docs/CHATGPT_PROJECT_DOCUMENTATION_LIST.md`.
   - **Legal/content:** Replace `docs/06-operations/legal/` and `docs/06-operations/content/` **contents** with Project B’s legal docs and copy. Folder structure and READMEs stay.
   - **08-status contents:** Clear or replace `docs/08-status/` **contents** (reports, logs, examination outputs, qa). Feature requests live in GitHub Project, not docs. Replace with Project B’s status. Keep folder structure and READMEs; reset ACTIVE_WORK.md / ACTIVE_WORK_LOG.md / INTEGRITY_CHECK_LOG.md / IMPLEMENTATION_VERIFICATION.md to empty/minimal per their formats.

3. **Set up GitHub coordination** (required for agent coordination):
   - Run [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md) for repo creation and MAIN_REPO_PATH
   - Run [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)
   - Create GitHub Project "Agent Coordination"; create labels; configure views (Directives, Active Work, etc.)
   - Verify with checklist in the setup guide

4. **Leave everything else as-is** in the copy: AI.md, docs/README.md, docs/01-orientation/GETTING_STARTED.md, docs/06-operations/ (all SOPs and processes), docs/07-guides/templates/, and 08-status folder structure and READMEs.

5. **Commit** the new project’s state in its own repo. The **original** repo (e.g. sourceapp) is untouched.

**Result:** New project has full infrastructure + GitHub coordination + empty or new project-specific layers. Original repo is unchanged.

---

## Workflow 2: Use a separate “template” repo (recommended for many projects)

Use when you want a **clean template** that has **only** infrastructure + placeholders, so you never touch project-specific content from an existing project. You maintain one “docs-infrastructure” (or “project-docs-template”) repo; every new project = clone that repo.

### Steps

1. **Create a template repo** (once) that contains:
   - All **infrastructure** paths (see [Explicit path list](#explicit-path-list) below).
   - **PROJECT_IDENTITY template:** `docs/01-orientation/PROJECT_IDENTITY.md` with generic placeholders (e.g. “Project name”, “project-slug”, “Repository role”, “Related repositories”) and the standard “Infrastructure vs project-specific” and “Applying this structure” sections.
   - **Optional placeholders for 02–05:** Empty or minimal `docs/02-canon/`, `docs/03-architecture/`, `docs/04-behavior/`, `docs/05-integrations/` with the **file names** your projects use (e.g. GUIDELINES.md, SYSTEM_MAP.md, AUTHENTICATION_FLOWS.md) and one-line “Fill per project” content, so every new project already has the right files to edit.
   - **Optional 08-status placeholders:** Empty ACTIVE_WORK, ACTIVE_WORK_LOG, INTEGRITY_CHECK_LOG, IMPLEMENTATION_VERIFICATION, plus folder structure and READMEs for screen-examination, backend-examination, user-flows, reports, qa. Feature requests → GitHub Project.

2. **To start a new project:** Clone the template repo (e.g. `git clone <template-repo-url> <new-project-name>`). No wiping—you’re cloning a repo that has no project-specific content.

3. **In the clone:** Set MAIN_REPO_PATH in AI.md. Fill PROJECT_IDENTITY, then fill 02–05 and 08-status contents for the new project. Infrastructure files are already correct.

4. **Set up GitHub coordination** (required for agent coordination): Run [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md) and [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md). Create GitHub Project, create labels, configure views. Verify with checklist.

5. **When you improve infrastructure** (e.g. new SOP, better AI.md): Update the **template repo**. New projects get the update when they’re cloned. Existing projects can occasionally merge from the template or copy changed infrastructure paths by hand.

**Result:** You never wipe any project. The template is “infrastructure only”; each project is a clone of that template plus its own project-specific content.

---

## Explicit path list

Use this list to script “copy only infrastructure” or “replace only project-specific” or to hand-check what to copy vs replace.

### Infrastructure — copy as-is; do not wipe

Copy these into a new project unchanged (except PROJECT_IDENTITY in Workflow 1, which you overwrite with new content). When building a template repo, include exactly these.

| Path | Notes |
|------|--------|
| `AI.md` | Root AI guide |
| `ADOPTION.md` | One-liner adoption prompt for architect |
| `docs/README.md` | Navigation spine |
| `docs/01-orientation/COORDINATION_MODEL.md` | What lives in GitHub Project vs docs; full flow and reasoning |
| `docs/01-orientation/PROJECT_DOCUMENTATION_HIERARCHY.md` | What the hierarchy is, why it exists, how it works |
| `docs/01-orientation/GLOBAL_AI_AGENT_RULES.md` | Project-agnostic AI agent rules; optimized for context consumption |
| `docs/01-orientation/GETTING_STARTED.md` | How to use the structure |
| `docs/02-canon/GUIDELINES.md` | Project-agnostic AI behavior and documentation expectations |
| `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md` | This file |
| `docs/06-operations/` | Entire tree (processes, examination, coordination, usage, conventions, management, legal/content **folders** and READMEs; legal/content **file contents** are project-specific—see below) |
| `docs/07-guides/templates/` | Entire tree |
| `docs/07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md` | Recurring auth flow (core infra) |
| `docs/07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md` | Standard Render setup (core infra) |
| `docs/07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md` | GitHub Projects + Discussions setup for agent coordination (run when creating new project) |
| `docs/07-guides/templates/render.yaml.template` | Render render.yaml template (core infra) |
| `docs/06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md` | Repo creation and MAIN_REPO_PATH setup |
| `docs/08-status/` | **Structure only:** ACTIVE_WORK.md, ACTIVE_WORK_LOG.md, INTEGRITY_CHECK_LOG.md, IMPLEMENTATION_VERIFICATION.md (can use empty/minimal template), CURRENT_STATUS.md (minimal), folder layout and READMEs for screen-examination/, backend-examination/, user-flows/, reports/, qa/, scope-of-work/, agent-coordination/. Feature requests → GitHub Project. |
| `docs/CHATGPT_PROJECT_DOCUMENTATION_LIST.md` | Optional but useful |

**Template repo:** Include the above. For PROJECT_IDENTITY, include a **template** version with placeholder name/slug/role and the standard “Infrastructure vs project-specific” and “Applying this structure” sections.

### Project-specific — replace or fill in the new project

These paths **exist in every project** (same file names and layout) but **content** is different per project. In a new project, overwrite or fill with that project’s data. In a template repo, you can add **placeholder** files (same names, minimal “Fill per project” body) so the layout is ready.

| Path | Notes |
|------|--------|
| `docs/01-orientation/PROJECT_IDENTITY.md` | Overwrite with new project’s name, slug, role, relations |
| `docs/02-canon/` | All files—canon, guidelines, audit, etc. Content is project-specific |
| `docs/03-architecture/` | All files—system map, db, state machines, etc. |
| `docs/04-behavior/` | All files—auth, API matrix, UI integration points, design system, etc. |
| `docs/05-integrations/` | All files—external APIs, webhooks, etc. |
| `docs/06-operations/legal/` | **Contents** of files (e.g. COOKIE_POLICY.md). Folder + README are infrastructure |
| `docs/06-operations/content/` | **Contents** of files. Folder + README are infrastructure |
| Feature requests | Live in GitHub Project (label `coordination:feature-request`); not in docs (desync across worktrees) |
| `docs/08-status/screen-examination/*` | Report files (not the README) |
| `docs/08-status/backend-examination/*` | Report files (not the README) |
| `docs/08-status/user-flows/*` | Report files (not the README) |
| `docs/08-status/reports/*` | Report log and report files (not the README) |
| `docs/08-status/qa/*` | QA TODO and work log contents |
| `docs/08-status/scope-of-work/SCOPE_OF_WORK.md` | SOW content (scope, deliverables, work items)—project-specific; folder and README are infrastructure |
| `docs/08-status/*.md` | ACTIVE_WORK, ACTIVE_WORK_LOG, INTEGRITY_CHECK_LOG, IMPLEMENTATION_VERIFICATION, CURRENT_STATUS, MIGRATION_EXECUTIVE_OVERVIEW—reset to empty/minimal or project-specific as needed |

---

## Scripting a “new project from infrastructure”

You can use the path list above to:

1. **Create “infrastructure only” bundle:** Copy only the infrastructure paths into a tarball or new folder. Untar or copy that into a new project root when starting a project.
2. **“New project” script:** From this repo, run a script that (1) creates `<new-project-dir>`, (2) copies only infrastructure paths into it, (3) writes a PROJECT_IDENTITY template and optional empty 02–05 placeholders, (4) leaves 08-status structure with empty logs. You never modify this repo; the script only reads from it and writes to the new directory.

Example (pseudocode):

```text
# Pseudocode: bootstrap-new-project.sh <new-project-dir>
# 1. Create <new-project-dir>
# 2. Copy: AI.md, docs/README.md, docs/01-orientation/{GETTING_STARTED,BOOTSTRAP_NEW_PROJECT}.md, docs/06-operations/, docs/07-guides/templates/
# 3. Copy docs/08-status/ structure and READMEs; write empty ACTIVE_WORK, ACTIVE_WORK_LOG, INTEGRITY_CHECK_LOG, IMPLEMENTATION_VERIFICATION, CURRENT_STATUS from templates
# 4. Write docs/01-orientation/PROJECT_IDENTITY.md from template (with placeholder name/slug/role)
# 5. Create empty docs/02-canon, 03-architecture, 04-behavior, 05-integrations (or copy placeholder files if you maintain them)
# 6. Create empty docs/08-status/ screen-examination, backend-examination, user-flows, reports, qa with READMEs only. Feature requests → GitHub Project.
```

The explicit path list above is the source of truth for what goes in step 2 (infrastructure) vs what is “empty placeholder” in steps 4–6 (project-specific slots).

---

## Summary

- **Do not wipe** the repo you’re building. To start a new project, **copy** (clone or folder copy) to a new location, then replace only project-specific content **in the copy**.
- **Infrastructure** = reuse as-is. **Project-specific** = same file names and layout in every project, different content; replace or fill in the new project.
- **Easiest long-term:** Maintain a **template repo** with only infrastructure + PROJECT_IDENTITY template + optional 02–05 and 08-status placeholders. Every new project = clone template, fill PROJECT_IDENTITY and 02–05. No wiping, no carrying over old project data.

---

**See also:** [Project Identity](PROJECT_IDENTITY.md) (defines name/slug and infrastructure vs project-specific), [Getting Started](GETTING_STARTED.md) (what we keep vs what you fill in).

**Last updated:** 2026-01-27
