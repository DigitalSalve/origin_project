# Project Documentation Infrastructure — Template

**Purpose:** Brand-agnostic documentation infrastructure for software projects. Copy this entire structure to new projects and fill in project-specific content.

**Created:** 2026-01-28  
**Source:** Extracted from SourceApp/Cheqpass projects  
**Version:** 1.0

---

## What This Contains

This is a **complete documentation infrastructure base** with:

- ✅ **AI behavior system** (AI.md + reading order)
- ✅ **8-layer documentation hierarchy** (Orientation → Canon → Architecture → Behavior → Integrations → Operations → Guides → Status)
- ✅ **23 SOPs** (examination, coordination, processes, management, usage)
- ✅ **3 infrastructure patterns** (authentication, deployment, architecture)
- ✅ **3 templates** (feature notes, SOPs, render.yaml)
- ✅ **Folder structure** for tracking (reports, logs). Feature requests → GitHub Project.

**Total:** ~45 reusable files + complete folder structure

---

## How to Use

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

## What to Keep vs Replace

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

## Documentation

**Full guidance:** See `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md`

**Infrastructure inventory:** See `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md`

---

## Included Infrastructure Patterns

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

## Quick Reference

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

## Projects Using This Infrastructure

- **SourceApp** (civic engagement platform)
- **Cheqpass** (fintech SaaS)
- **Your Project** (copy and fill!)

---

**Ready to use. Copy to your project and start filling in your content.**
