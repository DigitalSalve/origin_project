# Documentation

**Navigation spine for all project documentation.**

This document routes you to the right documentation layer. **AI.md points here for everything about the docs spine:** required reading order, layer intents, discovery flow, and all process (feature requests, before making changes, gaps, scope, documentation completion, coordination, creating new docs, when to stop and ask, infrastructure template maintenance). Read layers in order (1–8) when onboarding; jump to specific layers when working on specific areas.

**Project identity:** This project’s name, slug, repository role, and the boundary between **infrastructure** (reusable) and **project-specific** content are defined in [Project Identity](01-orientation/PROJECT_IDENTITY.md). Use that document when referring to this project by name or when discerning what to keep vs replace when reusing this structure.
- [Core Infrastructure Inventory](01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) — **Complete catalog** of all reusable infrastructure files (~45 files): processes, SOPs, templates, infrastructure guides. Use when identifying what to copy to new projects vs what to replace with project-specific content.

This structure is a **base for future projects**. You keep the AI instructions and this general structure; you fill in project-specific content in layers 2–5 (and elsewhere) when you apply the base to a new project.

**Documentation and repo integrity (flow):** Discovery runs **AI.md → this file (docs/README) → layer**. Operations (Layer 6: SOPs, processes, legal/content folders) define *how* to perform tasks; reports and status (Layer 8) are *outputs* of those operations. Each SOP that produces a report specifies its output location (e.g. Screen Examination → `08-status/screen-examination/`, Backend Examination → `08-status/backend-examination/`). New docs or reports must be linked here in the correct layer so the chain **operations → reports → spine** stays intact.

**Daily integrity check:** The [Repo Integrity Check SOP](06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md) is run **at least once per calendar day**. If [INTEGRITY_CHECK_LOG](08-status/INTEGRITY_CHECK_LOG.md) does **not** show an entry for **today** (YYYY-MM-DD), run the Repo Integrity Check SOP **before** performing the next operation (feature work, new docs, examination, migration step, etc.). See the [Process and behavior (reference)](#process-and-behavior-reference) table below for all steps.

---

## Required reading order (AI and automation)

You must read documentation in this order before making changes:

1. **AI.md** — Entry point and behavior rules
2. **This file (docs/README.md)** — Navigation spine to all documentation
3. **docs/01-orientation/** — How to approach this repository
4. **docs/02-canon/** — System truth, invariants, compliance, audit rules
5. **docs/03-architecture/** — Internal construction, state, data, boundaries
6. **docs/04-behavior/** — User, admin, UI, and system interactions
7. **docs/05-integrations/** — Third-party APIs, contracts, reference material
8. **docs/06-operations/** — Deployment, audits, lifecycle, documentation process
9. **docs/07-guides/** — Post-completion feature documentation and SOPs
10. **docs/08-status/** — Time-bound progress, test reports, unresolved issues

Skipping layers increases the risk of incorrect assumptions.

---

## Discovery flow

- **Discovery:** AI.md → this file → layer (1–8). Never rely on undocumented or unlinked docs; if it is not linked in this file, it is not part of the official flow.
- **Operations (Layer 6):** SOPs and processes define *how* to perform tasks; all are linked from this file Layer 6.
- **Reports and decisions (Layer 8):** Examination reports, status, and the Decision Log live in `08-status/`. Feature requests live in the GitHub Project (not docs). Each report type is produced by a specific SOP; outputs are linked in Layer 8 below.
- **Correlation:** Follow the SOP that governs the task, write outputs to the location the SOP specifies, and link those outputs here so the chain **operations → reports → spine** stays intact.

**Daily integrity check:** [Repo Integrity Check SOP](06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md) is run **at least once per calendar day**. If [INTEGRITY_CHECK_LOG](08-status/INTEGRITY_CHECK_LOG.md) has **no** entry for **today** (YYYY-MM-DD), run the SOP **before** the next operation.

---

## Process and behavior (reference)

Use this file as the single pointer for all docs-spine process. Details live in the linked SOPs and docs.

| Topic | Where to go |
|-------|-------------|
| **Where static vs dynamic lives** | [Coordination Model](01-orientation/COORDINATION_MODEL.md) — Static (docs) = read first; Dynamic (Project) = act |
| **When user requests new behavior** | Create Project item with `coordination:feature-request` first; do not implement directly. Full lifecycle: [Documentation Process](06-operations/processes/DOCUMENTATION_PROCESS.md) |
| **Feature request creation** | Create Project item with `coordination:feature-request`; spec in item body; full lifecycle: [Documentation Process](06-operations/processes/DOCUMENTATION_PROCESS.md) |
| **Before making changes** | Daily integrity check (above); [Active Work SOP](06-operations/coordination/ACTIVE_WORK_SOP.md) (check-out/check-in); create feature request before implementing new behavior; read affected layer; confirm no canon violated |
| **When finding implementation gaps** | [Implementation Verification SOP](06-operations/coordination/IMPLEMENTATION_VERIFICATION_SOP.md); track gaps in Project feature-request items and [IMPLEMENTATION_VERIFICATION](08-status/IMPLEMENTATION_VERIFICATION.md) |
| **When identifying/classifying gaps** | [Gap Identification SOP](06-operations/processes/GAP_IDENTIFICATION_SOP.md); scope boundaries: [Project Scope](02-canon/PROJECT_SCOPE.md) |
| **When scope questions arise** | [Project Scope](02-canon/PROJECT_SCOPE.md) is authoritative (create per project); use Gap Identification SOP to classify and update Scope |
| **When creating/updating Scope of Work** | [Scope of Work SOP](06-operations/processes/SCOPE_OF_WORK_SOP.md); output: [Scope of Work](08-status/scope-of-work/SCOPE_OF_WORK.md) |
| **Documentation completion (pre-commit)** | [Documentation Agent SOP](06-operations/coordination/DOCUMENTATION_AGENT_SOP.md); feature completion steps in [Documentation Process](06-operations/processes/DOCUMENTATION_PROCESS.md) |
| **Coordination (check-out/check-in)** | [Active Work SOP](06-operations/coordination/ACTIVE_WORK_SOP.md); GitHub Project Active Work view or [ACTIVE_WORK](08-status/ACTIVE_WORK.md), [ACTIVE_WORK_LOG](08-status/ACTIVE_WORK_LOG.md) |
| **Agent coordination (directives, Q&A)** | [Agent Coordination SOP](06-operations/coordination/AGENT_COORDINATION_SOP.md); GitHub Project Directives view and `coordination:q-a` items. Setup: [GitHub Coordination Setup Guide](07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) |
| **Creating new documentation** | See [Maintenance](#maintenance) below; add new items to this file in the correct layer; never create orphaned docs |
| **When to stop and ask** | When change affects funds/identity/audit; docs contradict; file seems to belong to multiple layers; behavior not documented; unsure if data belongs in existing file |
| **Infrastructure template maintenance** | [Infrastructure Template Sync SOP](06-operations/processes/INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md), [Infrastructure Template Audit SOP](06-operations/processes/INFRASTRUCTURE_TEMPLATE_AUDIT_SOP.md); blank template: `~/Github/blank/` |
| **Adopt blank into existing project** | [Infrastructure Template Adoption SOP](06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md) — Apply blank to existing project; merge, don't wipe |

---

## When to Read This

- **First time here?** Start with [Layer 1: Orientation](#layer-1-orientation)
- **Making code changes?** Read Layers 2–4 before coding
- **Integrating with external providers?** Jump to [Layer 5: Integrations](#layer-5-integrations)
- **Deploying or updating ops?** See [Layer 6: Operations](#layer-6-operations)
- **Checking current status?** See [Layer 8: Status and QA](#layer-8-status-and-qa)

---

## Layer 1: Orientation

**Purpose:** Navigation and how to approach this repository

**When to read:** First time onboarding, understanding project structure

- [Project Documentation Hierarchy](01-orientation/PROJECT_DOCUMENTATION_HIERARCHY.md) — **What it is, why it exists, how it works** — single explanation for humans and agents
- [Global AI Agent Rules](01-orientation/GLOBAL_AI_AGENT_RULES.md) — Project-agnostic rules for all AI agents; optimized for context consumption
- [Coordination Model](01-orientation/COORDINATION_MODEL.md) — **What lives where and why:** GitHub Project vs docs; worktree desync reasoning; full flow by artifact type; adoption and migration

- [Project Identity](01-orientation/PROJECT_IDENTITY.md) — **Source of truth** for this project’s name, slug, repository role, and infrastructure vs project-specific boundary; use when referring to this project by name or when reusing the structure
- [Core Infrastructure Inventory](01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) — **Complete catalog** of all reusable infrastructure files (~45 files): processes, SOPs, templates, infrastructure guides. Use when identifying what to copy to new projects vs what to replace with project-specific content.
- [Bootstrap New Project](01-orientation/BOOTSTRAP_NEW_PROJECT.md) — How to take this infrastructure to a new project **without wiping** the source: copy-this-repo vs template-repo workflows, explicit path list for scripting or manual copy
- [Getting Started Guide](01-orientation/GETTING_STARTED.md) — Setup and first steps
- [Documentation List for AI Projects](CHATGPT_PROJECT_DOCUMENTATION_LIST.md) — Essential documents to provide to ChatGPT/Claude projects for implementation understanding

---

## Layer 2: Canon and Mandates

**Purpose:** System truth, invariants, compliance, audit rules

**When to read:** Before making any changes that touch canon, compliance, or mandates

**These are mandates, not suggestions.** When applying this base to a project, fill this layer with project-specific canon.

- [Layer 2 README](02-canon/README.md) — **Fill per project.** Add `GUIDELINES.md`, `PROJECT_SCOPE.md`, and other canon docs. Link them here when created.

---

## Layer 3: Architecture

**Purpose:** Internal construction, state, data, and boundaries

**When to read:** Understanding how the system is built, database structure, state machines

- [Layer 3 README](03-architecture/README.md) — **Fill per project.** Add `SYSTEM_MAP.md`, `INTEGRATION_PLATFORMS.md`, and other architecture docs. Link them here when created.

**Typical docs:**
- `SYSTEM_MAP.md` — Quick index of how the system is wired
- [Integration Platforms](03-architecture/INTEGRATION_PLATFORMS.md) — Catalog of actively used and planned integration platforms (SendGrid, OpenAI, Render, AWS, Cloudflare, Plaid, etc.); settings and update procedures per [Integration Platforms Management SOP](06-operations/management/INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md)
- Database / entity relations and user flows
- State machine(s) for core flows
- Funds or value flow (if applicable)
- Admin access and support architecture
- Policy limits and user-status model (if applicable)

Add links here when created.

---

## Layer 4: Behavior

**Purpose:** User, admin, UI, and system interactions

**When to read:** Understanding how users interact with the system, UI flows, API endpoints

**Behavior docs (source of truth for flows, wiring, and UI standards):**
- [Layer 4 README](04-behavior/README.md) — **Fill per project.** Add behavior docs (e.g. AUTHENTICATION_FLOWS, API_MATRIX, INTENDED_USER_FLOWS). Link them here when created.
- *Typical:* AUTHENTICATION_FLOWS.md, COOKIES_AND_SESSION.md, API_MATRIX.md, UI_INTEGRATION_POINTS.md, DESIGN_SYSTEM.md, INTENDED_USER_FLOWS.md (per User Flow Identification SOP)
- [Cookies and Session](04-behavior/COOKIES_AND_SESSION.md) — Cookies used by sourceapp (`sourceapp_session`, `sourceapp_trusted_device`): how and when they’re used, attributes, env overrides; pointer to cookie policy (legal)
**Usage SOPs** (when behavior docs exist):
- [Authentication Flows — Usage SOP](06-operations/usage/AUTHENTICATION_FLOWS_USAGE_SOP.md)
- [API Matrix — Usage SOP](06-operations/usage/API_MATRIX_USAGE_SOP.md)
- [UI Integration Points — Usage SOP](06-operations/usage/UI_INTEGRATION_POINTS_USAGE_SOP.md)
- [Design System — Usage SOP](06-operations/usage/DESIGN_SYSTEM_USAGE_SOP.md)
- [Subscription Tiers — Usage SOP](06-operations/usage/SUBSCRIPTION_TIERS_USAGE_SOP.md)

---

## Layer 5: Integrations

**Purpose:** Third-party APIs, contracts, and reference material

**When to read:** Integrating with external providers

- [Layer 5 README](05-integrations/README.md) — **Fill per project.** Add integration docs. Link them here when created.

**Typical docs:**

- `INTEGRATION_REQUIREMENTS.md` — Backend implementation and API wiring
- Provider-specific implementation docs (per external service)
- `WEBHOOK_CONTRACTS.md` — Webhook verification and payload expectations
- Subfolders per provider for API references (as needed)

Add links here when created.

---

## Layer 6: Operations

**Purpose:** Deployment, audits, archival, documentation process, migration process

**When to read:** Deploying, updating documentation, managing lifecycle, screen examination, migrating from source (when applicable)

**Structure:** Operations are grouped in subfolders. See [06-operations/README.md](06-operations/README.md) for the index.

- [Documentation Process](06-operations/processes/DOCUMENTATION_PROCESS.md) — How to keep docs in sync with code
- [Migration Process](06-operations/processes/MIGRATION_PROCESS.md) — How migration from source to target is performed: source left read-only, all adaptation work in this project
- [Migration: Work in Isolation of Render](06-operations/processes/MIGRATION_LOCAL_PHASE.md) — Migration work that can be done without Render (local backend, DB, render.yaml, env template); use before or in parallel with connecting Render
- [Scope of Work SOP](06-operations/processes/SCOPE_OF_WORK_SOP.md) — Create a Scope of Work from expected project requirements and define/work to complete them; project-agnostic culmination of planning (canon, architecture, behavior, examinations, feature requests); output: `08-status/scope-of-work/SCOPE_OF_WORK.md`
- [Infrastructure Template Sync SOP](06-operations/processes/INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md) — **Infrastructure maintenance.** How to sync infrastructure file updates from active projects to blank template (`~/Github/blank/`); ensures template stays current when infrastructure improves
- [Infrastructure Template Audit SOP](06-operations/processes/INFRASTRUCTURE_TEMPLATE_AUDIT_SOP.md) — **Infrastructure maintenance.** How to audit blank template for drift and schedule syncs; run monthly to detect infrastructure improvements not yet in template
- [Agent Customization SOP](06-operations/processes/AGENT_CUSTOMIZATION_SOP.md) — Customize generic agents with project-specific context (paths, domain, APIs, tech stack)
- [Custom Agent Creation SOP](06-operations/processes/CUSTOM_AGENT_CREATION_SOP.md) — Create domain-specific or workflow-specific custom agents
- [Agent Testing and Validation SOP](06-operations/processes/AGENT_TESTING_VALIDATION_SOP.md) — Test and validate agent functionality with Max Mode and Task tool
- [Infrastructure Template Adoption SOP](06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md) — **Adopt blank into existing project.** Apply blank infrastructure to an existing project; copy infrastructure, merge vs replace, adapt MAIN_REPO_PATH and PROJECT_IDENTITY; use when instructing an architect/agent to adopt blank
- [Active Work SOP](06-operations/coordination/ACTIVE_WORK_SOP.md) — Check-out/check-in of files and areas before and after work; format, conflict handling, archive policy for ACTIVE_WORK and ACTIVE_WORK_LOG
- [06-operations/architecture/](06-operations/architecture/README.md) — **Project-specific.** Add architecture SOPs here (e.g. frontend-backend architecture). See [Infrastructure: Optimal Architecture](07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md) for reusable pattern.
- [Documentation Agent SOP](06-operations/coordination/DOCUMENTATION_AGENT_SOP.md) — **Pre-commit:** Ensure documentation is up to date before committing file changes; checklist for subsystem docs, System Map, feature notes, discoverability
- [Screen Examination SOP](06-operations/examination/SCREEN_EXAMINATION_SOP.md) — Systematic process for examining screens to identify broken or stubbed content
- [Backend Examination SOP](06-operations/examination/BACKEND_EXAMINATION_SOP.md) — Systematic process for examining the backend, identifying key components and processes, and creating a structured outline (working / not working / needing implementation)
- [User Flow Identification SOP](06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md) — Identifying intended user flows across the entire site or app map and documenting the full intended experience; output is `docs/04-behavior/INTENDED_USER_FLOWS.md` (or equivalent) and optional dated reports in `08-status/user-flows/`
- [Implementation Verification SOP](06-operations/coordination/IMPLEMENTATION_VERIFICATION_SOP.md) — Verifying implementation status, tracking open/resolved questions in IMPLEMENTATION_VERIFICATION.md, and ensuring gaps are tracked in Project feature-request items; procedure for when and how to run verification
- [File Naming Conventions](06-operations/conventions/FILE_NAMING_CONVENTIONS.md) — Unified file naming conventions for docs, reports, feature requests, templates, folders, and migrations
- [Report Creation and Delivery SOP](06-operations/conventions/REPORT_CREATION_AND_DELIVERY_SOP.md) — How to create, place, log, and deliver reports; where reports go (SOP-owned folders vs. `08-status/reports/`); lifecycle for reports in `reports/`
- [Integration Platforms Management SOP](06-operations/management/INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md) — Managing integration-platform settings, update procedures, and adding or retiring platforms (add/change settings, upgrades, Layer 5 docs)
- [Legal Documents SOP](06-operations/management/LEGAL_DOCUMENTS_SOP.md) — Managing the legal documents folder (terms of service, privacy policy, cookie policy, disclaimers); adding, updating, retiring
- [Content Documents SOP](06-operations/management/CONTENT_DOCUMENTS_SOP.md) — Managing the content documents folder (homepage, landing, section copy); adding, updating, retiring
- [Repo Integrity Check SOP](06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md) — Daily repo integrity check (spine, layers, links, operations–reports); run at least once per day; if [INTEGRITY_CHECK_LOG](08-status/INTEGRITY_CHECK_LOG.md) has no entry for today, run before the next operation
- [Render API Access Setup](06-operations/RENDER_API_ACCESS_SETUP.md) — Enable programmatic access to Render (API key or CLI) for agents and scripts

**Operations folders (legal and content):**
- [Legal](06-operations/legal/) — Legal documents (terms of service, privacy policy, disclaimers). Managed per [Legal Documents SOP](06-operations/management/LEGAL_DOCUMENTS_SOP.md).
  - [Cookie Policy](06-operations/legal/COOKIE_POLICY.md) — User-facing cookie disclosure (GDPR/ePrivacy–aligned); technical basis in Layer 4 cookies doc when it exists.
- [Content](06-operations/content/) — Website and app page content (homepage, landing, section copy). Managed per [Content Documents SOP](06-operations/management/CONTENT_DOCUMENTS_SOP.md).

**Behavior doc usage SOPs** (how to use and maintain each Layer 4 behavior doc):
- [Authentication Flows — Usage SOP](06-operations/usage/AUTHENTICATION_FLOWS_USAGE_SOP.md)
- [API Matrix — Usage SOP](06-operations/usage/API_MATRIX_USAGE_SOP.md)
- [UI Integration Points — Usage SOP](06-operations/usage/UI_INTEGRATION_POINTS_USAGE_SOP.md)
- [Design System — Usage SOP](06-operations/usage/DESIGN_SYSTEM_USAGE_SOP.md)
- [Subscription Tiers — Usage SOP](06-operations/usage/SUBSCRIPTION_TIERS_USAGE_SOP.md)
- [Intended User Flows — Usage SOP](06-operations/usage/INTENDED_USER_FLOWS_USAGE_SOP.md) — When INTENDED_USER_FLOWS exists in `docs/04-behavior/`; creation per [User Flow Identification SOP](06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md)

---

## Layer 7: Guides

**Purpose:** Post-completion feature documentation and SOPs

**When to read:** After features are complete, for reference and templates

- [Templates: Feature Note](07-guides/templates/FEATURE_NOTE.md) — Template for documenting completed features
- [Templates: SOP Template](07-guides/templates/SOP_TEMPLATE.md) — Standardized template for creating new Standard Operating Procedures
- [Infrastructure: Authentication — Core Flow](07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) — **Core infrastructure.** Recurring authentication method (passwordless magic-link + backup code, web + mobile) so you don’t have to figure it out again for each project; implementation checklist included.
- [Infrastructure: Auth and Push Options](07-guides/INFRASTRUCTURE_AUTH_AND_PUSH_OPTIONS.md) — **Core infrastructure.** Auth mode (internal vs Firebase), admin toggles (internal_auth_enabled, firebase_auth_enabled), and push (FCM).
- [Infrastructure: Render — Standard Setup](07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md) — **Core infrastructure.** Standard Render deployment (static + API + worker + database); template in [render.yaml.template](07-guides/templates/render.yaml.template). Copy, replace project slug, deploy.
- [GitHub Coordination Setup Guide](07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) — **Core infrastructure.** Set up GitHub Projects and Discussions as agent coordination layer when starting a new project. Run when bootstrapping per [Bootstrap New Project](01-orientation/BOOTSTRAP_NEW_PROJECT.md).

---

## Layer 8: Status and QA

**Purpose:** Time-bound progress, test reports, unresolved issues, feature requests

**When to read:** Checking current implementation status, QA results, planned features

**These documents are time-bound and may become outdated.**

**Status:**
- [Current Status](08-status/CURRENT_STATUS.md) — Latest development updates and recent changes
- [GitHub Coordination Migration Plan](08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md) — For existing projects moving coordination from docs to GitHub. **New projects:** use [GitHub Coordination Setup Guide](07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) instead.
- *(Add migration docs per project when applicable: MIGRATION_EXECUTIVE_OVERVIEW.md, MIGRATION_READINESS_ASSESSMENT.md, BACKEND_ADAPTATION_PLAN.md, etc.)*
- [Active Work](08-status/ACTIVE_WORK.md) — **REQUIRED:** Check-out files before editing, check-in when done (procedure: [Active Work SOP](06-operations/coordination/ACTIVE_WORK_SOP.md))
- [Active Work Log](08-status/ACTIVE_WORK_LOG.md) — **REQUIRED:** Append all check-out/check-in entries here (append-only, never remove); format and archive per [Active Work SOP](06-operations/coordination/ACTIVE_WORK_SOP.md)
- [Integrity Check Log](08-status/INTEGRITY_CHECK_LOG.md) — **REQUIRED (daily):** Append-only log of repo integrity checks. If there is **no** entry for **today** (YYYY-MM-DD), run [Repo Integrity Check SOP](06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md) **before** the next operation. See [Process and behavior (reference)](#process-and-behavior-reference) above.
- [Implementation Verification](08-status/IMPLEMENTATION_VERIFICATION.md) — Open and resolved implementation verification questions; procedure: [Implementation Verification SOP](06-operations/coordination/IMPLEMENTATION_VERIFICATION_SOP.md)
- [Decision Log](08-status/decisions/DECISION_LOG.md) — Time-stamped record of major decisions with rationale and supersession pointers

**Feature Requests:**
- **GitHub Project → Feature Requests view** — Feature requests live in the Project (label `coordination:feature-request`); spec in item body. See [Documentation Process](06-operations/processes/DOCUMENTATION_PROCESS.md).

**QA:**
- [QA: TODO](08-status/qa/QA_TODO.md) — QA checklist
- [QA: Work Log](08-status/qa/QA_WORK_LOG.md) — QA work tracking

**Documentation Reports:**
- **Create reports:** Project items with label `coordination:report`; optionally promote to [08-status/reports/](08-status/reports/) after review. See [Coordination Model](01-orientation/COORDINATION_MODEL.md).
- [Reports Guidelines](08-status/reports/README.md) — Folder structure and lifecycle; **how to create and deliver:** [Report Creation and Delivery SOP](06-operations/conventions/REPORT_CREATION_AND_DELIVERY_SOP.md)

**Screen Examination:**
- Reports under [08-status/screen-examination/](08-status/screen-examination/) — Per [Screen Examination SOP](06-operations/examination/SCREEN_EXAMINATION_SOP.md); add links here when reports exist

**Backend Examination:**
- Reports under [08-status/backend-examination/](08-status/backend-examination/) — Per [Backend Examination SOP](06-operations/examination/BACKEND_EXAMINATION_SOP.md); add links here when reports exist

**User Flow Identification:**
- Authoritative doc: `docs/04-behavior/INTENDED_USER_FLOWS.md` (created/updated per [User Flow Identification SOP](06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md))
- Optional dated reports under [08-status/user-flows/](08-status/user-flows/) — Audit runs; per [User Flow Identification SOP](06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md)

**Scope of Work:**
- [Scope of Work](08-status/scope-of-work/SCOPE_OF_WORK.md) — Living SOW from expected requirements; procedure: [Scope of Work SOP](06-operations/processes/SCOPE_OF_WORK_SOP.md); optional snapshots in [08-status/scope-of-work/](08-status/scope-of-work/)

---

## Quick Reference

### By Task

**I want to...**
- **Use the recurring auth flow (passwordless magic-link + backup code, web + mobile)** → See [Infrastructure: Authentication — Core Flow](07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md)
- **Configure auth mode (internal vs Firebase) and push (FCM)** → See [Infrastructure: Auth and Push Options](07-guides/INFRASTRUCTURE_AUTH_AND_PUSH_OPTIONS.md)
- **Use the standard Render setup (static + API + worker + DB)** → See [Infrastructure: Render — Standard Setup](07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md); template: [render.yaml.template](07-guides/templates/render.yaml.template)
- **Set up agent coordination (GitHub Projects)** → See [GitHub Coordination Setup Guide](07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md); run when creating new project
- **Understand deployment workflow** → See [Deployment Workflow](06-operations/DEPLOYMENT_WORKFLOW.md) — No local deployment; commit → push to GitHub → Render auto-syncs and deploys
- **Set up Render API access (for agents/scripts)** → See [Render API Access Setup](06-operations/RENDER_API_ACCESS_SETUP.md) — API key or CLI for programmatic access
- **Onboard to the project** → Start with [Layer 1](01-orientation/)
- **Understand system rules** → Read [Layer 2](02-canon/)
- **Understand database and architecture** → See Layer 3 (e.g. SYSTEM_MAP, database docs)
- **Wire UI to API** → See Layer 4 (e.g. API Matrix, UI Integration Points)
- **Integrate external providers** → See [Layer 5](05-integrations/)
- **Follow documentation process** → See [Documentation Process](06-operations/processes/DOCUMENTATION_PROCESS.md)
- **Ensure docs are up to date before commit** → See [Documentation Agent SOP](06-operations/coordination/DOCUMENTATION_AGENT_SOP.md) (pre-commit checklist)
- **Plan or run migration** → See [Migration Process](06-operations/processes/MIGRATION_PROCESS.md); **migration work without Render** → See [Migration: Work in Isolation of Render](06-operations/processes/MIGRATION_LOCAL_PHASE.md)
- **Create or update Scope of Work** → See [Scope of Work SOP](06-operations/processes/SCOPE_OF_WORK_SOP.md); output in [08-status/scope-of-work/](08-status/scope-of-work/)
- **Identify gaps (feature, infrastructure, scope)** → See [Gap Identification SOP](06-operations/processes/GAP_IDENTIFICATION_SOP.md); scope boundaries: [Project Scope](02-canon/PROJECT_SCOPE.md)
- **Check current status** → See [Current Status](08-status/CURRENT_STATUS.md)
- **Examine screens** → See [Screen Examination SOP](06-operations/examination/SCREEN_EXAMINATION_SOP.md); reports in [08-status/screen-examination/](08-status/screen-examination/)
- **Examine backend** → See [Backend Examination SOP](06-operations/examination/BACKEND_EXAMINATION_SOP.md); reports in [08-status/backend-examination/](08-status/backend-examination/)
- **Document intended user flows** → See [User Flow Identification SOP](06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md); output in `docs/04-behavior/INTENDED_USER_FLOWS.md` (or equivalent); optional reports in [08-status/user-flows/](08-status/user-flows/)
- **Manage legal or content docs** → See [Legal](06-operations/legal/) folder and [Legal Documents SOP](06-operations/management/LEGAL_DOCUMENTS_SOP.md); [Content](06-operations/content/) folder and [Content Documents SOP](06-operations/management/CONTENT_DOCUMENTS_SOP.md)
- **Apply file naming** → See [File Naming Conventions](06-operations/conventions/FILE_NAMING_CONVENTIONS.md)
- **Create or deliver a report** → See [Report Creation and Delivery SOP](06-operations/conventions/REPORT_CREATION_AND_DELIVERY_SOP.md); create Project items with `coordination:report`; reports in SOP-owned folders or [08-status/reports/](08-status/reports/) for promoted snapshots
- **Run repo integrity check / verify daily integrity** → See [Repo Integrity Check SOP](06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md); check [Integrity Check Log](08-status/INTEGRITY_CHECK_LOG.md) for today—if no entry, run the SOP before the next operation

### By Role

**I am a...**
- **New engineer** → Read layers 1–4 in order
- **Backend developer** → Focus on layers 2–3, 5
- **Frontend developer** → Focus on layers 2, 4
- **DevOps engineer** → Focus on layers 2, 6
- **QA engineer** → Focus on layers 2, 4, 8
- **Product manager** → Focus on layers 1, 2, 4, 8
- **Documentation agent** → Run [Documentation Agent SOP](06-operations/coordination/DOCUMENTATION_AGENT_SOP.md) before committing file changes; ensure subsystem docs, System Map, feature notes, and discoverability are up to date

---

## Documentation Principles

1. **Layers must not mix concerns** — Architecture docs don't describe behavior; behavior docs don't describe architecture
2. **Canon is truth** — Layer 2 documents are mandates, not suggestions
3. **Status is time-bound** — Layer 8 documents may become outdated; check dates
4. **Read in order** — Layers 1–8 are ordered for a reason
5. **When in doubt, ask** — If a document's placement seems wrong, flag it

---

## Maintenance

When adding new documents or structures:

**Before creating a new file (REQUIRED):**
1. **Stop and ask:** "Does this data belong in an existing file or folder?"
2. **Search for similar content** — Check existing files/folders for similar data
3. **If data belongs elsewhere** — Add to the existing file instead of creating a new one
4. **If new file is truly needed** — Only then create it, after verifying no existing file serves this purpose
5. **Document your decision** — Note why a new file was needed vs. adding to existing

**When creating new documents or structures:**
1. **Place them in the correct layer folder**
2. **Immediately add a link here** with a brief description in the appropriate layer section
3. Include "When to read" guidance if it's a top-level document
4. **Verify discoverability** — ensure the path from `AI.md` → this file → your new structure is clear

**Critical:** Never create orphaned documentation. Orphaned documentation breaks the discovery chain. Always check if data belongs in an existing file before creating a new one.

---

## When finding implementation gaps

When making structural changes or verifying implementation status:

1. **Verify the gap** — Confirm through code review that a documented/planned feature is not implemented
2. **Update Project feature-request item** — Ensure the gap is listed in the "Not Yet Implemented" section of the relevant Project item (label `coordination:feature-request`)
3. **Update verification** — Record verification in `08-status/IMPLEMENTATION_VERIFICATION.md`
4. **Ask:** "Is this gap tracked in the feature-request item?"

**Critical:** Implementation gaps must be tracked in the Project feature-request items. Never leave gaps untracked.
