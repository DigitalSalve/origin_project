# Project Identity

**Purpose:** Single source of truth for this project's name, slug, repository role, and the boundary between **shared infrastructure** (reusable across projects) and **project-specific** content.

**TEMPLATE:** Replace all [PLACEHOLDERS] with your project's information.

---

## Project name and slug

| Field | Value | Use |
|-------|--------|-----|
| **Project name** | [Your Project Name] | Display name in docs, UI, and communication |
| **Project slug** | [your-project-slug] | Machine-friendly: paths, env vars, repo names, folder names |

## Main repo path

| Field | Value | Use |
|-------|--------|-----|
| **MAIN_REPO_PATH** | [Absolute path to project root, e.g. /Users/you/Github/my-project] | Set in AI.md and here. Agents use it for worktree redirects and doc paths. See [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md). |

**Example:**
- Project name: SourceApp
- Project slug: sourceapp

---

## Repository role

| Field | Value |
|-------|--------|
| **Repository role** | [client / service / worker / canon-docs / migration-target] |

**Options:**
- **client** — Non-authoritative client (collects evidence, displays state)
- **service** — Authoritative service (makes decisions, owns data)
- **worker** — Background worker (reacts, processes jobs)
- **canon-docs** — Documentation repository (defines truth)
- **migration-target** — Target of migration work

---

## Related repositories (this project)

| Relation | Repository / path | Notes |
|----------|-------------------|-------|
| **[Source / Related / Backend]** | [repo-name] | [Description of relationship] |
| **This repo** | [this-repo-name] | [What lives here] |

**Example:**
| Relation | Repository | Notes |
|----------|------------|-------|
| **Source (read-only)** | gop_app | Original app for migration reference |
| **This repo** | sourceapp | Migration target and documentation |

---

## Infrastructure vs project-specific

### Infrastructure (shared / reusable)

These files and folders are part of the **universal base**. Keep unchanged when copying to new projects.

| Location | Description |
|----------|-------------|
| **AI.md** | Entry point, behavior rules, reading order |
| **docs/README.md** | Navigation spine, layer structure |
| **docs/01-orientation/** | GETTING_STARTED, BOOTSTRAP_NEW_PROJECT, CORE_INFRASTRUCTURE_INVENTORY |
| **docs/06-operations/** | All SOPs, processes, conventions, management, usage |
| **docs/07-guides/templates/** | Feature note, SOP, render.yaml templates |
| **docs/07-guides/INFRASTRUCTURE_*.md** | Core infrastructure patterns (auth, render, architecture) |
| **docs/08-status/** | Folder structure, log formats, READMEs (not contents) |

---

### Project-specific (this project only)

Replace these with your project's content when copying infrastructure.

| Location | Description |
|----------|-------------|
| **docs/01-orientation/PROJECT_IDENTITY.md** | This file - update with your project info |
| **docs/02-canon/** | Your project's guidelines, scope, mandates |
| **docs/03-architecture/** | Your project's system map, database, state |
| **docs/04-behavior/** | Your project's auth flows, API matrix, UI |
| **docs/05-integrations/** | Your project's third-party integrations |
| **docs/06-operations/legal/** | Your legal documents (contents, not structure) |
| **docs/06-operations/content/** | Your content documents (contents, not structure) |
| **docs/06-operations/architecture/** | Your project-specific architecture SOPs |
| **docs/08-status/** | Your reports, logs, status (contents, not structure). Feature requests live in GitHub Project, not docs. |

---

## Applying this structure to a new project

See [Bootstrap New Project](BOOTSTRAP_NEW_PROJECT.md) for complete instructions.

**Quick version:**
1. Create repo and copy this template (see [New Project Repo Setup SOP](../06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md))
2. Set MAIN_REPO_PATH in AI.md
3. Update this file (PROJECT_IDENTITY.md) with new project info
4. Fill layers 2-5 with new project content
5. Reset layer 8 status (clear contents, keep structure)
6. Run [GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)
7. Leave infrastructure unchanged (layers 6-7, AI.md, docs/README.md)

---

**Last updated:** [Update when you fill this template]
