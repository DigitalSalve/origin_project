# Core Infrastructure Template - Manifest

**Created:** 2026-01-28  
**Source:** SourceApp/Cheqpass projects  
**Version:** 1.0  
**Total Files:** 54 markdown files + 23 directories  
**Last Sync:** 2026-01-28

---

## What This Template Contains

This is a **ready-to-use documentation infrastructure** extracted from production projects (SourceApp, Cheqpass) that provides:

1. **Complete AI behavior system** with reading order and coordination
2. **8-layer documentation hierarchy** (orientation through status)
3. **23 operational SOPs** for examination, migration, and management
4. **3 reusable infrastructure patterns** (auth, deployment, architecture)
5. **3 templates** (feature notes, SOPs, deployment config)
6. **Complete folder structure** for tracking work

---

## File Count by Category

| Category | Files | Purpose |
|----------|-------|---------|
| **Root** | 2 | AI.md + README |
| **Navigation** | 3 | Docs spine and orientation |
| **Orientation** | 3 | Getting started, bootstrap, inventory |
| **Processes** | 7 | Documentation, migration, SOW, gap ID, template sync & audit |
| **Coordination** | 4 | Active work, verification, integrity |
| **Examination** | 3 | Screen, backend, user flow SOPs |
| **Conventions** | 2 | File naming, report creation |
| **Management** | 3 | Legal, content, integration platforms |
| **Usage** | 6 | How to use behavior docs |
| **Templates** | 3 | Feature note, SOP, render.yaml |
| **Infrastructure** | 3 | Auth, render, architecture patterns |
| **Status Templates** | 8 | Active work, logs, verification, TODO |
| **Folder READMEs** | 5 | Feature requests, examinations, reports |
| **TOTAL** | 54 files | Complete infrastructure |

---

## Directory Structure

```
~/Github/blank/
├─ AI.md                                    (Root AI guide)
├─ README.md                                (Template overview)
├─ MANIFEST.md                              (This file)
└─ docs/
   ├─ README.md                             (Navigation spine)
   ├─ CHATGPT_PROJECT_DOCUMENTATION_LIST.md
   ├─ 01-orientation/
   │  ├─ PROJECT_IDENTITY.md                (TEMPLATE - fill per project)
   │  ├─ GETTING_STARTED.md
   │  ├─ BOOTSTRAP_NEW_PROJECT.md
   │  └─ CORE_INFRASTRUCTURE_INVENTORY.md
   ├─ 02-canon/                             (EMPTY - fill per project)
   ├─ 03-architecture/                      (EMPTY - fill per project)
   ├─ 04-behavior/                          (EMPTY - fill per project)
   ├─ 05-integrations/                      (EMPTY - fill per project)
   ├─ 06-operations/
   │  ├─ README.md
   │  ├─ DEPLOYMENT_WORKFLOW.md
   │  ├─ RENDER_API_ACCESS_SETUP.md
   │  ├─ processes/ (5 SOPs)
   │  ├─ coordination/ (4 SOPs)
   │  ├─ examination/ (3 SOPs)
   │  ├─ conventions/ (2 SOPs)
   │  ├─ management/ (3 SOPs)
   │  ├─ usage/ (6 SOPs)
   │  ├─ legal/ (folder + README - contents per project)
   │  ├─ content/ (folder + README - contents per project)
   │  └─ architecture/ (EMPTY - project-specific SOPs go here)
   ├─ 07-guides/
   │  ├─ templates/
   │  │  ├─ FEATURE_NOTE.md
   │  │  ├─ SOP_TEMPLATE.md
   │  │  └─ render.yaml.template
   │  ├─ INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md
   │  ├─ INFRASTRUCTURE_RENDER_STANDARD_SETUP.md
   │  └─ INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md
   └─ 08-status/
      ├─ ACTIVE_WORK.md (template)
      ├─ ACTIVE_WORK_LOG.md (template)
      ├─ INTEGRITY_CHECK_LOG.md (template)
      ├─ IMPLEMENTATION_VERIFICATION.md (template)
      ├─ CURRENT_STATUS.md (template)
      ├─ feature-requests/
      │  ├─ README.md
      │  ├─ FEATURE_REQUEST_TODO.md (template)
      │  └─ completed/
      ├─ screen-examination/ (+ README)
      ├─ backend-examination/ (+ README)
      ├─ user-flows/
      ├─ reports/ (+ README + REPORT_LOG)
      ├─ qa/
      ├─ decisions/
      └─ scope-of-work/
```

---

## What to Do Next

### Step 1: Copy to Your Project

```bash
# Copy entire blank folder to your project location
cp -r ~/Github/blank ~/Github/your-project-name
cd ~/Github/your-project-name
```

---

### Step 2: Update Project Identity

```bash
# Edit this file with your project info
edit docs/01-orientation/PROJECT_IDENTITY.md

Replace:
- [Your Project Name] → MyNewProject
- [your-project-slug] → mynewproject
- [Repository role] → client | service | etc.
```

---

### Step 3: Fill Project-Specific Layers

Create your project's content in:

- **Layer 2 (Canon):** `docs/02-canon/`
  - GUIDELINES.md (your AI behavior rules)
  - PROJECT_SCOPE.md (your product boundaries)
  - [Other canon docs]

- **Layer 3 (Architecture):** `docs/03-architecture/`
  - SYSTEM_MAP.md (your system components)
  - [Database, state machines, boundaries]

- **Layer 4 (Behavior):** `docs/04-behavior/`
  - AUTHENTICATION_FLOWS.md (your auth)
  - API_MATRIX.md (your endpoints)
  - [UI integration, design system]

- **Layer 5 (Integrations):** `docs/05-integrations/`
  - [Your third-party APIs, contracts]

- **Layer 8 (Status):** `docs/08-status/`
  - Create your feature requests
  - Update CURRENT_STATUS.md
  - Fill FEATURE_REQUEST_TODO.md

---

### Step 4: Start Using

1. Read `AI.md` (root)
2. Read `docs/README.md` (navigation)
3. Read `docs/01-orientation/GETTING_STARTED.md`
4. Follow SOPs as you build

---

## Infrastructure Patterns Included

### 1. Authentication Pattern
**Passwordless magic-link + backup code (web + mobile)**
- Email-based login
- 6-digit backup codes
- Device trust
- Session management

**File:** `docs/07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md`

---

### 2. Deployment Pattern
**Standard Render stack (static + API + worker + database)**
- Web service configuration
- Worker configuration
- Database setup
- Environment variables

**File:** `docs/07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md`

---

### 3. Architecture Pattern (NEW - 2026-01-28)
**Optimal frontend-backend for web + mobile with SEO**
- React/Next.js marketing (SEO)
- Flutter application (web + mobile code sharing)
- TypeScript backend (type safety)
- Deep link configuration
- User flow patterns

**File:** `docs/07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md`

---

## Validation

All infrastructure files are:
- ✅ Brand-agnostic (no hardcoded project names)
- ✅ Process-oriented (not implementation-specific)
- ✅ Linked in docs/README.md
- ✅ Ready to use immediately
- ✅ Tested across multiple projects

---

## Version History

### Version 1.0 (2026-01-28)
- Initial extraction from SourceApp/Cheqpass
- 54 markdown files
- 23 directories
- 3 infrastructure patterns
- Complete 8-layer structure
- Added infrastructure maintenance SOPs (sync + audit)
- Added version log tracking
- Reinforced AI behavior rules for template maintenance

---

## Support

**Documentation:**
- Read: `docs/01-orientation/GETTING_STARTED.md`
- Reference: `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md`
- Bootstrap: `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md`

**Questions:**
- All SOPs have detailed procedures
- Templates have examples
- Infrastructure patterns have rationale

---

**This template is ready to use. Copy to your project and start building!**

---

**Created:** 2026-01-28  
**Maintained by:** Architect Agent  
**Source Projects:** SourceApp (civic engagement), Cheqpass (fintech)
