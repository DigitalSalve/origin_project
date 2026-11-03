# origin_project

The documentation infrastructure template that PTheory grew from: a brand-agnostic base for software projects, extracted on 28 January 2026 and kept here as it stood.

## What it does

This is the predecessor of [PTheory](https://github.com/DigitalSalve/ptheory). It was extracted on 28 January 2026 from two production projects as a brand-agnostic documentation base, pushed to GitHub on 6 February 2026, and is kept as it stood. PTheory's entry-point file (`AI.md`), its eight-layer documentation hierarchy, its SOPs and its architect-and-subagent pattern all begin here. The name PTheory first appears on 22 March 2026; none of PTheory's own mechanics (the skill chain, workflow state, the exploration and operations split, contracts, gates) exist in this repo.

It gives a project:

- an AI behavior system: `AI.md` as the mandatory entry point, with a reading order
- an eight-layer documentation hierarchy: orientation, canon, architecture, behavior, integrations, operations, guides, status
- 23 SOPs for examination, coordination, processes, management and usage
- infrastructure patterns for authentication, deployment and architecture, and templates for feature notes, SOPs and a deployment manifest
- Cursor agents: an architect that orchestrates and never implements, plus backend, web, webapp and mobile; subagents report, the architect reviews and closes

## Quick start

```bash
# 1. Copy this folder to your new project
cp -r origin_project your-new-project

# 2. Set the project identity
#    edit docs/01-orientation/PROJECT_IDENTITY.md

# 3. Fill in the project-specific layers
#    docs/02-canon, 03-architecture, 04-behavior, 05-integrations, 08-status
```

For an existing project, tell the architect agent to adopt it: see [ADOPTION.md](ADOPTION.md).

## How it works

Agents read `AI.md` first and follow its reading order into the layers. The infrastructure layers (`AI.md`, `docs/README.md`, orientation, operations, guides, the status folder structure) stay unchanged across projects and are synced from this template; the project layers are filled per project. The original notes on what to keep and what to replace follow.

### What this contains

This is a **complete documentation infrastructure base** with:

- ✅ **AI behavior system** (AI.md + reading order)
- ✅ **8-layer documentation hierarchy** (Orientation → Canon → Architecture → Behavior → Integrations → Operations → Guides → Status)
- ✅ **23 SOPs** (examination, coordination, processes, management, usage)
- ✅ **3 infrastructure patterns** (authentication, deployment, architecture)
- ✅ **3 templates** (feature notes, SOPs, render.yaml)
- ✅ **Folder structure** for tracking (reports, logs). Feature requests → GitHub Project.

**Total:** ~45 reusable files + complete folder structure

---

### How to use

### New project (Quick Start):

```bash
# 1. Copy this folder to your new project
cp -r ~/Github/origin_project ~/Github/your-new-project

# 2. Update project identity
cd ~/Github/your-new-project
# Edit: docs/01-orientation/PROJECT_IDENTITY.md

# 3. Fill in project-specific layers
# - docs/02-canon/ (your system truth)
# - docs/03-architecture/ (your system design)
# - docs/04-behavior/ (your user flows)
# - docs/05-integrations/ (your third-party APIs)
# - docs/08-status/ (your reports, status). Feature requests → GitHub Project.
```

### Existing project (adoption):

Tell your architect: **Adopt origin_project into [project name]**. See [ADOPTION.md](ADOPTION.md).

---

### What to keep vs replace

### ✅ Keep Unchanged (Infrastructure):
- `AI.md` (root)
- `docs/README.md` (navigation spine)
- `docs/01-orientation/` (except PROJECT_IDENTITY - replace that)
- `docs/06-operations/` (all SOPs and processes)
- `docs/07-guides/` (templates and infrastructure patterns)
- `docs/08-status/` (folder structure, not contents)

### 📝 Fill With Your Project Data:
- `docs/01-orientation/PROJECT_IDENTITY.md` (your project name/slug)
- `docs/02-canon/` (your guidelines, scope, mandates)
- `docs/03-architecture/` (your system map, database, state)
- `docs/04-behavior/` (your auth flows, API matrix, UI)
- `docs/05-integrations/` (your third-party integrations)
- `docs/06-operations/legal/` (your terms, privacy policy)
- `docs/06-operations/content/` (your homepage copy)
- `docs/08-status/` contents (your features, reports, logs)

---

### Documentation

**Full guidance:** See `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md`

**Infrastructure inventory:** See `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md`

---

### Included Infrastructure Patterns

### 1. Authentication Pattern
**File:** `docs/07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md`  
**What:** Passwordless magic-link + backup code (web + mobile)  
**Use:** Don't re-derive auth for each project

### 2. Deployment Pattern
**File:** `docs/07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md`  
**What:** Render stack (static + API + worker + database)  
**Use:** Standard deployment across projects

### 3. Architecture Pattern
**File:** `docs/07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md`  
**What:** React marketing + Flutter app + TypeScript backend  
**Use:** Optimal SEO + consistency + code sharing

---

### Quick Reference

| Need | File |
|------|------|
| Start new project | `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md` |
| Adopt into existing project | [ADOPTION.md](ADOPTION.md) |
| List all infrastructure | `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md` |
| Navigation | `docs/README.md` |
| AI behavior rules | `AI.md` |
| Feature request process | `docs/06-operations/processes/DOCUMENTATION_PROCESS.md` |
| Examine screens | `docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md` |
| Deploy to Render | `docs/07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md` |

---

### Projects Using This Infrastructure

- a civic engagement platform
- a fintech SaaS
- **Your Project** (copy and fill!)

---

**Ready to use. Copy to your project and start filling in your content.**

## Status

Historical. This is the template as it stood on 6 February 2026, with the source projects' names replaced and one source-specific legal page removed. It is not maintained; PTheory superseded it on 31 March 2026.

## Credits

Written for Cursor and Claude Code agents. The two source projects it was extracted from are the author's own work.

## License

[MIT](LICENSE).
