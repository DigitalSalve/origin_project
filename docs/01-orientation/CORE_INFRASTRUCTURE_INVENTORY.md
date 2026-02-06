# Core Infrastructure Inventory

**Purpose:** Complete catalog of all reusable infrastructure files that are brand-agnostic and can be carried forward to new projects. This list defines what to **copy unchanged** vs what to **replace with project-specific content** when bootstrapping a new project.

**Created:** 2026-01-28  
**Status:** Active  
**Maintenance:** Update when new infrastructure is added

---

## Overview

This infrastructure base provides:
- Documentation hierarchy (8 layers)
- AI behavior rules and coordination
- SOPs for examination, migration, documentation, and operations
- Templates for feature requests, SOPs, and deployment
- Reusable patterns for authentication, deployment, and architecture

**Use:** When starting a new project, copy these files as-is and fill in project-specific content in designated layers.

**Infrastructure lock:** Only the Architect Agent may create, modify, or delete files listed in this inventory. Other agents must not change them; create a Project item (label `coordination:q-a`, target `agent:architect`) or invoke Architect. See AI.md Hard rules.

**When Architect changes infrastructure:** Include in the directive: **Update worktrees** — Agents in worktrees must commit any outstanding changes first, then pull (e.g. `git fetch origin main && git merge origin/main`). See AGENT_PROMPTS "For the Architect."

---

## Quick Reference

| Category | Count | Purpose |
|----------|-------|---------|
| **Root Files** | 1 | AI entry point |
| **Navigation** | 7 | Project hierarchy, global rules, coordination model, guidelines, docs spine, getting started |
| **Processes** | 7 | Repo setup, migration, documentation, SOW, gap identification, adoption |
| **Coordination** | 6 | Active work, integrity checks, verification, agent coordination, agent session hook |
| **Examination** | 3 | Screen, backend, user flow examination |
| **Conventions** | 2 | File naming, report creation |
| **Management** | 3 | Legal, content, integration platforms |
| **Usage SOPs** | 6 | How to use behavior docs |
| **Templates** | 3 | Feature notes, SOPs, render.yaml |
| **Infrastructure Guides** | 4 | Auth flow, auth/push options, Render setup, optimal architecture |
| **Structure Files** | ~12 | Status folder structure, READMEs |
| **TOTAL** | ~48 files | Complete infrastructure base |

---

## Category 1: Root Files (1 file)

### AI.md
**Location:** Repository root  
**Purpose:** Entry point for AI agents; defines behavior rules and hard rules; points to docs/README.md for docs spine, required reading order, and all process (feature requests, before making changes, coordination, etc.)  
**Reusable:** Yes (references PROJECT_IDENTITY for project name)  
**When copying:** Copy unchanged; it points to PROJECT_IDENTITY.md and docs/README.md

---

## Category 2: Navigation & Orientation (6 files)

### docs/01-orientation/PROJECT_DOCUMENTATION_HIERARCHY.md
**Location:** `docs/01-orientation/PROJECT_DOCUMENTATION_HIERARCHY.md`  
**Purpose:** Single explanation of the project documentation hierarchy: what it is, why it exists, how it works for humans and agents  
**Reusable:** Yes (structure is project-agnostic)  
**When copying:** Copy unchanged

### docs/01-orientation/GLOBAL_AI_AGENT_RULES.md
**Location:** `docs/01-orientation/GLOBAL_AI_AGENT_RULES.md`  
**Purpose:** Project-agnostic rules for all AI agents; optimized for context consumption; documents hierarchy, separation of concerns, coordination patterns  
**Reusable:** Yes (rules apply across all projects)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

### docs/01-orientation/COORDINATION_MODEL.md
**Location:** `docs/01-orientation/COORDINATION_MODEL.md`  
**Purpose:** Single source of truth for coordination model: what lives in GitHub Project vs docs; worktree desync reasoning; full flow by artifact type (feature requests, reports, directives); adoption and migration  
**Reusable:** Yes (model is project-agnostic)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

### docs/README.md
**Location:** `docs/README.md`  
**Purpose:** **Docs spine hub:** navigation to all 8 layers; required reading order; discovery flow; Process and behavior reference table (feature requests, before making changes, gaps, scope, coordination, etc.). AI.md points here for all docs-spine and process.  
**Reusable:** Yes (layer structure is fixed; links are placeholders)  
**When copying:** Copy unchanged; fill in layer 2-5 links per project

### docs/01-orientation/GETTING_STARTED.md
**Location:** `docs/01-orientation/GETTING_STARTED.md`  
**Purpose:** How to use this documentation structure  
**Reusable:** Yes (explains infrastructure vs project-specific)  
**When copying:** Copy unchanged

### docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md
**Location:** `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md`  
**Purpose:** How to take infrastructure to a new project without wiping source  
**Reusable:** Yes (explicit path list for copying)  
**When copying:** Copy unchanged

### docs/02-canon/GUIDELINES.md
**Location:** `docs/02-canon/GUIDELINES.md`  
**Purpose:** Project-agnostic AI behavior and documentation expectations; extended rules (code quality, security, testing)  
**Reusable:** Yes (content is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

---

## Category 3: Processes (8 files)

### DOCUMENTATION_PROCESS.md
**Location:** `docs/06-operations/processes/DOCUMENTATION_PROCESS.md`  
**Purpose:** How to keep docs in sync with code; feature request completion process  
**Reusable:** Yes (process is project-agnostic)  
**When copying:** Copy unchanged

### MIGRATION_PROCESS.md
**Location:** `docs/06-operations/processes/MIGRATION_PROCESS.md`  
**Purpose:** How migration from source to target is performed (source read-only)  
**Reusable:** Yes (generic process)  
**When copying:** Copy unchanged; applies if project has migration context

### MIGRATION_LOCAL_PHASE.md
**Location:** `docs/06-operations/processes/MIGRATION_LOCAL_PHASE.md`  
**Purpose:** Migration work that can be done without deployment platform  
**Reusable:** Yes (deployment-agnostic)  
**When copying:** Copy unchanged; applies if project has migration

### SCOPE_OF_WORK_SOP.md
**Location:** `docs/06-operations/processes/SCOPE_OF_WORK_SOP.md`  
**Purpose:** Create and manage Scope of Work from requirements  
**Reusable:** Yes (process is universal)  
**When copying:** Copy unchanged

### GAP_IDENTIFICATION_SOP.md
**Location:** `docs/06-operations/processes/GAP_IDENTIFICATION_SOP.md`  
**Purpose:** Identify and classify feature gaps, infrastructure gaps, scope gaps  
**Reusable:** Yes (classification process is universal)  
**When copying:** Copy unchanged

### INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md
**Location:** `docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md`  
**Purpose:** Sync infrastructure file updates from active projects to blank template  
**Reusable:** Yes (template maintenance process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

### INFRASTRUCTURE_TEMPLATE_AUDIT_SOP.md
**Location:** `docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_AUDIT_SOP.md`  
**Purpose:** Audit blank template for drift and schedule syncs (run monthly)  
**Reusable:** Yes (audit process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

### INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md
**Location:** `docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md`  
**Purpose:** Adopt blank infrastructure into an existing project; copy infrastructure, merge vs replace, adapt placeholders, set up GitHub coordination  
**Reusable:** Yes (adoption process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

### NEW_PROJECT_REPO_SETUP_SOP.md
**Location:** `docs/06-operations/processes/NEW_PROJECT_REPO_SETUP_SOP.md`  
**Purpose:** Repo creation, MAIN_REPO_PATH setup, and coordination bootstrap for new projects  
**Reusable:** Yes (process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-01-28

---

### AGENT_CUSTOMIZATION_SOP.md
**Location:** `docs/06-operations/processes/AGENT_CUSTOMIZATION_SOP.md`  
**Purpose:** Customize generic agent definitions with project-specific context (paths, domain vocabulary, API structure, tech stack)  
**Reusable:** Yes (customization process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-02-05

### CUSTOM_AGENT_CREATION_SOP.md
**Location:** `docs/06-operations/processes/CUSTOM_AGENT_CREATION_SOP.md`  
**Purpose:** Create domain-specific or workflow-specific custom agents beyond the standard 5 (architect, backend, web, webapp, mobile)  
**Reusable:** Yes (creation process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-02-05

### AGENT_TESTING_VALIDATION_SOP.md
**Location:** `docs/06-operations/processes/AGENT_TESTING_VALIDATION_SOP.md`  
**Purpose:** Test and validate agent definitions work correctly with Task tool, Max Mode, and Architect coordination  
**Reusable:** Yes (testing process is universal)  
**When copying:** Copy unchanged  
**Created:** 2026-02-05

## Category 4: Coordination (6 files)

### ACTIVE_WORK_SOP.md
**Location:** `docs/06-operations/coordination/ACTIVE_WORK_SOP.md`  
**Purpose:** Check-out/check-in process for file coordination  
**Reusable:** Yes (coordination format is universal)  
**When copying:** Copy unchanged

### DOCUMENTATION_AGENT_SOP.md
**Location:** `docs/06-operations/coordination/DOCUMENTATION_AGENT_SOP.md`  
**Purpose:** Pre-commit documentation checklist  
**Reusable:** Yes (checklist is generic)  
**When copying:** Copy unchanged

### IMPLEMENTATION_VERIFICATION_SOP.md
**Location:** `docs/06-operations/coordination/IMPLEMENTATION_VERIFICATION_SOP.md`  
**Purpose:** Verify implementation status, track gaps  
**Reusable:** Yes (verification process is universal)  
**When copying:** Copy unchanged

### REPO_INTEGRITY_CHECK_SOP.md
**Location:** `docs/06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md`  
**Purpose:** Daily repo integrity check (spine, layers, links)  
**Reusable:** Yes (integrity checks are structural)  
**When copying:** Copy unchanged

### AGENT_COORDINATION_SOP.md
**Location:** `docs/06-operations/coordination/AGENT_COORDINATION_SOP.md`  
**Purpose:** Agent-to-agent communication — questions, alignment, administrative coordination; GitHub Project is canonical channel  
**Reusable:** Yes (coordination format is universal)  
**When copying:** Copy unchanged; output: GitHub Project items with label `coordination:q-a`

### AGENT_SESSION_HOOK_SOP.md
**Location:** `docs/06-operations/coordination/AGENT_SESSION_HOOK_SOP.md`  
**Purpose:** Defines the hook command ("Report to the agent team") — when user gives this command, agent reads GitHub Project Directives view, responds to coordination items, then does work  
**Reusable:** Yes (command and procedure are universal)  
**When copying:** Copy unchanged

---

## Category 5: Examination (3 files)

### SCREEN_EXAMINATION_SOP.md
**Location:** `docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md`  
**Purpose:** Systematic process for examining screens (stubbed content, API integration, mock data)  
**Reusable:** Yes (examination process is universal)  
**When copying:** Copy unchanged

### BACKEND_EXAMINATION_SOP.md
**Location:** `docs/06-operations/examination/BACKEND_EXAMINATION_SOP.md`  
**Purpose:** Systematic process for examining backend (working/not working/needing implementation)  
**Reusable:** Yes (examination process is universal)  
**When copying:** Copy unchanged

### USER_FLOW_IDENTIFICATION_SOP.md
**Location:** `docs/06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md`  
**Purpose:** Identify and document intended user flows across entire application  
**Reusable:** Yes (flow identification process is universal)  
**When copying:** Copy unchanged

---

## Category 6: Conventions (2 files)

### FILE_NAMING_CONVENTIONS.md
**Location:** `docs/06-operations/conventions/FILE_NAMING_CONVENTIONS.md`  
**Purpose:** Unified file naming for docs, reports, feature requests, migrations  
**Reusable:** Yes (conventions are universal)  
**When copying:** Copy unchanged

### REPORT_CREATION_AND_DELIVERY_SOP.md
**Location:** `docs/06-operations/conventions/REPORT_CREATION_AND_DELIVERY_SOP.md`  
**Purpose:** How to create, place, log, and deliver reports  
**Reusable:** Yes (report process is universal)  
**When copying:** Copy unchanged

---

## Category 7: Management (3 files)

### LEGAL_DOCUMENTS_SOP.md
**Location:** `docs/06-operations/management/LEGAL_DOCUMENTS_SOP.md`  
**Purpose:** Managing legal documents folder (terms, privacy, disclaimers)  
**Reusable:** Yes (management process is universal)  
**When copying:** Copy unchanged

### CONTENT_DOCUMENTS_SOP.md
**Location:** `docs/06-operations/management/CONTENT_DOCUMENTS_SOP.md`  
**Purpose:** Managing content documents folder (homepage, landing, copy)  
**Reusable:** Yes (management process is universal)  
**When copying:** Copy unchanged

### INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md
**Location:** `docs/06-operations/management/INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md`  
**Purpose:** Managing integration platform settings (SendGrid, AWS, etc.)  
**Reusable:** Yes (management process is universal)  
**When copying:** Copy unchanged

---

## Category 8: Usage SOPs (6 files)

### AUTHENTICATION_FLOWS_USAGE_SOP.md
**Location:** `docs/06-operations/usage/AUTHENTICATION_FLOWS_USAGE_SOP.md`  
**Purpose:** How to use and maintain AUTHENTICATION_FLOWS.md  
**Reusable:** Yes (usage process is universal)  
**When copying:** Copy unchanged

### API_MATRIX_USAGE_SOP.md
**Location:** `docs/06-operations/usage/API_MATRIX_USAGE_SOP.md`  
**Purpose:** How to use and maintain API_MATRIX.md  
**Reusable:** Yes (usage process is universal)  
**When copying:** Copy unchanged

### UI_INTEGRATION_POINTS_USAGE_SOP.md
**Location:** `docs/06-operations/usage/UI_INTEGRATION_POINTS_USAGE_SOP.md`  
**Purpose:** How to use and maintain UI_INTEGRATION_POINTS.md  
**Reusable:** Yes (usage process is universal)  
**When copying:** Copy unchanged

### DESIGN_SYSTEM_USAGE_SOP.md
**Location:** `docs/06-operations/usage/DESIGN_SYSTEM_USAGE_SOP.md`  
**Purpose:** How to use and maintain DESIGN_SYSTEM.md  
**Reusable:** Yes (usage process is universal)  
**When copying:** Copy unchanged

### SUBSCRIPTION_TIERS_USAGE_SOP.md
**Location:** `docs/06-operations/usage/SUBSCRIPTION_TIERS_USAGE_SOP.md`  
**Purpose:** How to use and maintain SUBSCRIPTION_TIERS.md  
**Reusable:** Yes (usage process is universal)  
**When copying:** Copy unchanged

### INTENDED_USER_FLOWS_USAGE_SOP.md
**Location:** `docs/06-operations/usage/INTENDED_USER_FLOWS_USAGE_SOP.md`  
**Purpose:** How to use and maintain INTENDED_USER_FLOWS.md  
**Reusable:** Yes (usage process is universal)  
**When copying:** Copy unchanged

---

## Category 9: Templates (3 files)

### FEATURE_NOTE.md
**Location:** `docs/07-guides/templates/FEATURE_NOTE.md`  
**Purpose:** Template for documenting completed features  
**Reusable:** Yes (template structure is fixed)  
**When copying:** Copy unchanged

### SOP_TEMPLATE.md
**Location:** `docs/07-guides/templates/SOP_TEMPLATE.md`  
**Purpose:** Template for creating new SOPs  
**Reusable:** Yes (template structure is fixed)  
**When copying:** Copy unchanged

### render.yaml.template
**Location:** `docs/07-guides/templates/render.yaml.template`  
**Purpose:** Template for Render deployment configuration  
**Reusable:** Yes (replace project slug when using)  
**When copying:** Copy unchanged

---

## Category 10: Infrastructure Guides (4 files)

### INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md
**Location:** `docs/07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md`  
**Purpose:** Recurring authentication method (passwordless magic-link + backup code, web + mobile)  
**Reusable:** Yes — **Core infrastructure**  
**Benefit:** Don't re-derive auth flow for each project  
**When copying:** Copy unchanged; implementation checklist included

### INFRASTRUCTURE_RENDER_STANDARD_SETUP.md
**Location:** `docs/07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md`  
**Purpose:** Standard Render deployment (static + API + worker + database)  
**Reusable:** Yes — **Core infrastructure**  
**Benefit:** Don't re-derive deployment stack for each project  
**When copying:** Copy unchanged; use with render.yaml.template

### INFRASTRUCTURE_AUTH_AND_PUSH_OPTIONS.md
**Location:** `docs/07-guides/INFRASTRUCTURE_AUTH_AND_PUSH_OPTIONS.md`  
**Purpose:** Auth mode (internal vs Firebase) and push (FCM); admin toggles for internal_auth_enabled and firebase_auth_enabled  
**Reusable:** Yes — **Core infrastructure**  
**Benefit:** Don't re-derive auth-mode and push decisions for each project  
**When copying:** Copy unchanged; implementation checklists included

### INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md
**Location:** `docs/07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md`  
**Purpose:** Optimal architecture for web + mobile apps with SEO (React marketing + Flutter app + TypeScript backend)  
**Reusable:** Yes — **Core infrastructure**  
**Benefit:** Don't re-derive architecture decisions for each project  
**When copying:** Copy unchanged; includes deep links, user flows, tech selection  
**Created:** 2026-01-28

### GITHUB_COORDINATION_SETUP_GUIDE.md
**Location:** `docs/07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md`  
**Purpose:** Complete guide for setting up GitHub Projects and Discussions as agent coordination layer when starting a new project  
**Reusable:** Yes — **Core infrastructure**  
**Benefit:** New projects have worktree-consistent coordination from day one; no re-deriving logistics  
**When copying:** Copy unchanged; run when bootstrapping new project per BOOTSTRAP_NEW_PROJECT.md  
**Created:** 2026-01-28

### GITHUB_COORDINATION_MIGRATION_PLAN.md
**Location:** `docs/08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md`  
**Purpose:** Migration plan for existing projects moving coordination from docs to GitHub  
**Reusable:** Yes (reference; execution is project-specific)  
**When copying:** Copy unchanged; applies to projects with docs-based coordination  
**Created:** 2026-01-28

---

## Category 11: Status Structure (12 files)

These define the **structure and format** for Layer 8 (Status). Contents are project-specific, but the structure is reusable.

### Status Files (Format/Structure)

| File | Purpose | Reusable? |
|------|---------|-----------|
| `docs/08-status/ACTIVE_WORK.md` | Check-out coordination (format) | ✅ Structure (contents project-specific) |
| `docs/08-status/ACTIVE_WORK_LOG.md` | Append-only check-out/check-in log (format) | ✅ Structure (reset for new project) |
| `docs/08-status/INTEGRITY_CHECK_LOG.md` | Daily repo integrity checks (format) | ✅ Structure (reset for new project) |
| `docs/08-status/IMPLEMENTATION_VERIFICATION.md` | Implementation questions (format) | ✅ Structure (reset for new project) |
| `docs/08-status/CURRENT_STATUS.md` | Latest development updates | ⚠️ Template only (contents project-specific) |
| `docs/08-status/feature-requests/FEATURE_REQUEST_TODO.md` | **Deprecated.** Feature requests live in GitHub Project. | ⚠️ Deprecated |

### Folder Structure (Reusable)

| Folder | Purpose | Copy? |
|--------|---------|-------|
| `docs/08-status/feature-requests/` | **Deprecated for active work.** Feature requests live in GitHub Project. Folder may hold README or legacy completed snapshots. | ⚠️ Deprecated |
| `docs/08-status/screen-examination/` | Screen examination reports | ✅ Folder + README (not contents) |
| `docs/08-status/backend-examination/` | Backend examination reports | ✅ Folder + README (not contents) |
| `docs/08-status/user-flows/` | User flow reports | ✅ Folder structure |
| `docs/08-status/reports/` | General reports. Create Project items with `coordination:report`; optionally promote to folder. | ✅ Folder + README (Project-based) |
| `docs/08-status/qa/` | QA tracking | ✅ Folder structure |
| `docs/08-status/decisions/` | Decision log | ✅ Folder structure |
| `docs/08-status/scope-of-work/` | Scope of Work | ✅ Folder structure |
| `docs/08-status/agent-coordination/` | Agent-to-agent communication. AI.md and docs are main-repo only; MAIN_REPO_PATH at top of AI.md. Includes SUBAGENT_TRANSITION_PLAN for Architect + Cursor subagents. | ✅ Folder + README + AGENT_DIRECTIVES + AGENT_COORDINATION_LOG + AGENT_PROMPTS + AGENT_COORDINATION_QUICK_START + SUBAGENT_TRANSITION_PLAN |

---

## Category 12: Legal & Content (Folder Structure)

### Legal Folder
**Location:** `docs/06-operations/legal/`  
**Reusable:** Folder structure + README + LEGAL_DOCUMENTS_SOP  
**Not reusable:** Actual legal document contents (terms, privacy, etc.)  
**When copying:** Copy folder and README; clear or replace contents

### Content Folder
**Location:** `docs/06-operations/content/`  
**Reusable:** Folder structure + README + CONTENT_DOCUMENTS_SOP  
**Not reusable:** Actual content documents (homepage copy, etc.)  
**When copying:** Copy folder and README; clear or replace contents

---

## Category 13: Additional Operations Files (3 files)

### DEPLOYMENT_WORKFLOW.md
**Location:** `docs/06-operations/DEPLOYMENT_WORKFLOW.md`  
**Purpose:** How deployment works (commit → push → auto-deploy)  
**Reusable:** Yes (if using Render; adjust if using other platforms)  
**When copying:** Copy unchanged or adapt for deployment platform

### RENDER_API_ACCESS_SETUP.md
**Location:** `docs/06-operations/RENDER_API_ACCESS_SETUP.md`  
**Purpose:** Enable Render API access for agents/scripts  
**Reusable:** Yes (Render-specific but universal process)  
**When copying:** Copy unchanged

### README.md (Operations)
**Location:** `docs/06-operations/README.md`  
**Purpose:** Index of all operations documents  
**Reusable:** Yes (structure; links filled per project)  
**When copying:** Copy unchanged

---

## Complete Infrastructure File List

### To Copy Unchanged (Core Infrastructure):

```
Root:
├─ AI.md
├─ ADOPTION.md (one-liner adoption prompt)
├─ .cursor/rules/ (read-ai-md-first.mdc)
└─ .cursor/agents/ (Cursor subagents: architect, backend, web, webapp, mobile)

Layer 1 (Orientation):
├─ docs/README.md
├─ docs/01-orientation/COORDINATION_MODEL.md
├─ docs/01-orientation/PROJECT_DOCUMENTATION_HIERARCHY.md
├─ docs/01-orientation/GLOBAL_AI_AGENT_RULES.md
├─ docs/01-orientation/GETTING_STARTED.md
├─ docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md
└─ docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md (this file)

Layer 6 (Operations):
├─ docs/06-operations/README.md
├─ docs/06-operations/DEPLOYMENT_WORKFLOW.md
├─ docs/06-operations/RENDER_API_ACCESS_SETUP.md
├─ docs/06-operations/processes/
│   ├─ DOCUMENTATION_PROCESS.md
│   ├─ MIGRATION_PROCESS.md
│   ├─ MIGRATION_LOCAL_PHASE.md
│   ├─ SCOPE_OF_WORK_SOP.md
│   └─ GAP_IDENTIFICATION_SOP.md
├─ docs/06-operations/coordination/
│   ├─ ACTIVE_WORK_SOP.md
│   ├─ AGENT_COORDINATION_SOP.md
│   ├─ AGENT_SESSION_HOOK_SOP.md
│   ├─ DOCUMENTATION_AGENT_SOP.md
│   ├─ IMPLEMENTATION_VERIFICATION_SOP.md
│   └─ REPO_INTEGRITY_CHECK_SOP.md
├─ docs/06-operations/examination/
│   ├─ SCREEN_EXAMINATION_SOP.md
│   ├─ BACKEND_EXAMINATION_SOP.md
│   └─ USER_FLOW_IDENTIFICATION_SOP.md
├─ docs/06-operations/conventions/
│   ├─ FILE_NAMING_CONVENTIONS.md
│   └─ REPORT_CREATION_AND_DELIVERY_SOP.md
├─ docs/06-operations/management/
│   ├─ LEGAL_DOCUMENTS_SOP.md
│   ├─ CONTENT_DOCUMENTS_SOP.md
│   └─ INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md
├─ docs/06-operations/usage/
│   ├─ AUTHENTICATION_FLOWS_USAGE_SOP.md
│   ├─ API_MATRIX_USAGE_SOP.md
│   ├─ UI_INTEGRATION_POINTS_USAGE_SOP.md
│   ├─ DESIGN_SYSTEM_USAGE_SOP.md
│   ├─ SUBSCRIPTION_TIERS_USAGE_SOP.md
│   └─ INTENDED_USER_FLOWS_USAGE_SOP.md
└─ docs/06-operations/legal/ (folder + README only)
└─ docs/06-operations/content/ (folder + README only)

Layer 7 (Guides):
├─ docs/07-guides/templates/
│   ├─ FEATURE_NOTE.md
│   ├─ SOP_TEMPLATE.md
│   └─ render.yaml.template
├─ docs/07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md
├─ docs/07-guides/INFRASTRUCTURE_AUTH_AND_PUSH_OPTIONS.md
├─ docs/07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md
└─ docs/07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md

Layer 8 (Status - Structure Only):
├─ docs/08-status/ (folder structure)
├─ docs/08-status/ACTIVE_WORK.md (format, reset contents)
├─ docs/08-status/ACTIVE_WORK_LOG.md (format, reset contents)
├─ docs/08-status/INTEGRITY_CHECK_LOG.md (format, reset contents)
├─ docs/08-status/IMPLEMENTATION_VERIFICATION.md (format, reset contents)
├─ docs/08-status/feature-requests/ (folder + README, deprecated—use GitHub Project)
├─ docs/08-status/screen-examination/ (folder + README)
├─ docs/08-status/backend-examination/ (folder + README)
├─ docs/08-status/user-flows/ (folder)
├─ docs/08-status/reports/ (folder + README; Project-based)
├─ docs/08-status/qa/ (folder)
├─ docs/08-status/decisions/ (folder)
├─ docs/08-status/scope-of-work/ (folder)
└─ docs/08-status/agent-coordination/ (folder + README + AGENT_DIRECTIVES + AGENT_COORDINATION_LOG + AGENT_PROMPTS + AGENT_COORDINATION_QUICK_START + SUBAGENT_TRANSITION_PLAN)
```

**Total: ~50 infrastructure files + folder structure**

---

## To Replace or Fill (Project-Specific):

```
Layer 1 (Orientation):
└─ docs/01-orientation/PROJECT_IDENTITY.md (replace with new project)

Layer 2 (Canon):
└─ docs/02-canon/* (fill with project canon, guidelines, scope)

Layer 3 (Architecture):
└─ docs/03-architecture/* (fill with project system map, DB, state machines)

Layer 4 (Behavior):
└─ docs/04-behavior/* (fill with project auth flows, API matrix, UI integration)

Layer 5 (Integrations):
└─ docs/05-integrations/* (fill with project third-party APIs, contracts)

Layer 6 (Operations - Contents Only):
├─ docs/06-operations/legal/* (replace legal doc contents)
├─ docs/06-operations/content/* (replace content doc contents)
└─ docs/06-operations/architecture/* (project-specific SOPs like SOURCEAPP_*, CHEQPASS_*)

Layer 8 (Status - Contents Only):
├─ docs/08-status/CURRENT_STATUS.md (replace with project status)
├─ Feature requests → GitHub Project (not docs)
├─ docs/08-status/screen-examination/* (replace with project reports)
├─ docs/08-status/backend-examination/* (replace with project reports)
└─ All other status contents (reset or fill per project)
```

---

## Infrastructure Components by Type

### Documentation System (15 files)
- Navigation spine
- Layer structure definitions
- Reading order enforcement
- Discoverability chain (AI.md → README → layers)

### Coordination System (4 files)
- Active work tracking
- Implementation verification
- Daily integrity checks
- Documentation agent checklist

### Examination System (3 files)
- Screen examination
- Backend examination
- User flow identification

### Process System (5 files)
- Documentation process
- Migration process
- Scope of Work creation
- Gap identification
- Local phase migration

### Management System (3 files)
- Legal documents
- Content documents
- Integration platforms

### Convention System (2 files)
- File naming
- Report creation

### Usage System (6 files)
- How to use behavior docs
- Maintain API Matrix, Auth Flows, etc.

### Template System (3 files)
- Feature notes
- SOPs
- Render deployment

### Infrastructure Patterns (3 files)
- Authentication flow (magic-link)
- Render deployment stack
- Optimal frontend-backend architecture

---

## How to Use This Inventory

### For New Project:

1. **Copy all infrastructure files** (listed above)
2. **Replace PROJECT_IDENTITY.md** with new project's identity
3. **Fill layers 2-5** with new project's content
4. **Reset layer 8 status** (clear contents, keep structure)
5. **Replace legal/content** (keep folders, replace contents)

### For Infrastructure Improvements:

1. **Identify file** in this inventory
2. **Update in source project** (e.g., SourceApp)
3. **Copy to other projects** (e.g., Cheqpass)
4. **Update this inventory** if adding new infrastructure

### For Determining Reusability:

- **In Layer 7 with "INFRASTRUCTURE_" prefix** → Core infrastructure ✅
- **In Layer 6 /processes, /coordination, /conventions** → Core infrastructure ✅
- **In Layer 6 /architecture with project name** → Project-specific ❌
- **In Layers 2-5** → Project-specific ❌
- **In Layer 8 (contents)** → Project-specific ❌

---

## Validation Checklist

When adding new infrastructure, verify:

- [ ] File is brand-agnostic (no hardcoded project names)
- [ ] Process/pattern is reusable across projects
- [ ] Placed in correct layer (Layer 6 or 7 for infrastructure)
- [ ] Linked in `docs/README.md`
- [ ] Uses "this project" or "see PROJECT_IDENTITY" for project references
- [ ] Added to this inventory document
- [ ] Marked as "Core infrastructure" if in Layer 7

---

## Recent Additions

### 2026-01-28:
- ✅ Added: `GITHUB_COORDINATION_SETUP_GUIDE.md` (Layer 7)
  - Complete setup guide for GitHub Projects + Discussions as agent coordination
  - Run when bootstrapping new project; ensures worktree-consistent coordination from day one
- ✅ Added: `GITHUB_COORDINATION_MIGRATION_PLAN.md` (Layer 8)
  - Migration plan for existing projects moving from docs to GitHub
- ✅ Updated: `AGENT_COORDINATION_SOP.md`, `AGENT_SESSION_HOOK_SOP.md` — GitHub-first versions
- ✅ Updated: `ACTIVE_WORK_SOP.md` — GitHub mode (Project items) + docs mode
- ✅ Updated: `docs/08-status/agent-coordination/README.md` — "Coordination lives in GitHub"
- ✅ Updated: `AI.md` — Coordination section points to GitHub Project
- ✅ Updated: `BOOTSTRAP_NEW_PROJECT.md` — Step: Run GitHub Coordination Setup Guide
- ✅ Added: `PROJECT_DOCUMENTATION_HIERARCHY.md` (Layer 1)
  - Single explanation of the docs hierarchy: what it is, why it exists, how it works
  - For humans and agents
- ✅ Added: `GLOBAL_AI_AGENT_RULES.md` (Layer 1)
  - Project-agnostic rules for all AI agents; optimized for context consumption
  - Documents hierarchy, separation of concerns, worktree redirects, coordination patterns
- ✅ Added: `AGENT_COORDINATION_SOP.md`, `AGENT_SESSION_HOOK_SOP.md` (Layer 6)
  - Agent-to-agent communication; hook command "Report to the agent team"
- ✅ Added: `docs/08-status/agent-coordination/` (Layer 8)
  - Folder structure: README, AGENT_DIRECTIVES, AGENT_COORDINATION_LOG, BREAKOUT_TEMPLATE, AGENT_PROMPTS, AGENT_COORDINATION_QUICK_START
- ✅ Added: `INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md` (Layer 7)
  - Brand-agnostic pattern for React marketing + Flutter app + TypeScript backend
  - Includes deep links, SEO rationale, technology comparisons
  - Documented as core infrastructure for future projects

---

## Summary

**Total Infrastructure Files:** ~50 files  
**Total Folder Structures:** ~12 folders  
**Reusable Patterns:** 3 major patterns (Auth, Render, Architecture)  
**Templates:** 3 templates (Feature Note, SOP, render.yaml)  
**SOPs:** 23 operational procedures  

**All infrastructure is:**
- ✅ Brand-agnostic
- ✅ Process-oriented
- ✅ Reusable across projects
- ✅ Documented and linked
- ✅ Ready to copy to new projects

---

**Related Documentation:**
- [PROJECT_IDENTITY](PROJECT_IDENTITY.md) — What's infrastructure vs project-specific
- [BOOTSTRAP_NEW_PROJECT](BOOTSTRAP_NEW_PROJECT.md) — How to copy infrastructure to new project
- [GETTING_STARTED](GETTING_STARTED.md) — How to use this structure

---

**Last Updated:** 2026-01-28  
**Maintained By:** Architect Agent  
**Update Trigger:** When new infrastructure is added or structure changes
