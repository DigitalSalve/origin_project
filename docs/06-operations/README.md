# Layer 6: Operations

**Purpose:** Deployment, audits, archival, documentation process, migration process. This folder holds processes, SOPs, and managed content (legal, content) that define *how* to perform operations. All are linked from `docs/README.md` Layer 6; this index shows the subfolder structure.

---

## Subfolder structure

| Subfolder | Contents |
|-----------|----------|
| **processes/** | Documentation Process, Migration Process, [Migration: Work in Isolation of Render](processes/MIGRATION_LOCAL_PHASE.md), [Database Migrations SOP](processes/DATABASE_MIGRATIONS_SOP.md), Scope of Work SOP, Gap Identification SOP, Product Boundary Breach SOP |
| **examination/** | Screen Examination SOP, Backend Examination SOP, User Flow Identification SOP |
| **coordination/** | Active Work SOP, Repo Integrity Check SOP, Implementation Verification SOP, Documentation Agent SOP, Production Deployment Validation SOP, Manual Sync Trigger SOP, Commit Changes SOP, Operational Asset Creation and Reinforcement SOP |
| **usage/** | Behavior-doc usage SOPs (Authentication Flows, API Matrix, UI Integration Points, Design System, Subscription Tiers, Intended User Flows) |
| **conventions/** | File Naming Conventions, Report Creation and Delivery SOP |
| **management/** | Integration Platforms Management SOP, Environment Variables Management SOP, Legal Documents SOP, Content Documents SOP |
| **legal/** | Legal documents folder (cookie policy, etc.). Managed per [Legal Documents SOP](management/LEGAL_DOCUMENTS_SOP.md). |
| **content/** | Content documents folder (homepage, landing, section copy). Managed per [Content Documents SOP](management/CONTENT_DOCUMENTS_SOP.md). |

---

## Quick links (by category)

**Processes:** [Documentation Process](processes/DOCUMENTATION_PROCESS.md) · [Migration Process](processes/MIGRATION_PROCESS.md) · [Migration: Work in Isolation of Render](processes/MIGRATION_LOCAL_PHASE.md) · [Database Migrations SOP](processes/DATABASE_MIGRATIONS_SOP.md) · [Scope of Work SOP](processes/SCOPE_OF_WORK_SOP.md) · [Gap Identification SOP](processes/GAP_IDENTIFICATION_SOP.md) · [Product Boundary Breach SOP](processes/PRODUCT_BOUNDARY_BREACH_SOP.md)

**Deployment:** [Deployment Workflow](DEPLOYMENT_WORKFLOW.md) — No local deployment; commit → push to GitHub → Render auto-syncs and deploys · [Render Services Index](RENDER_SERVICES_INDEX.md) — **Canonical reference** for all Render services (sourceapp-api, sourceapp-web, sourceapp-webapp, sourceapp-worker, sourceapp-db); where each is defined and documented · [ENV_VARS_RENDER](ENV_VARS_RENDER.md) — List of env vars to add and **where** (sourceapp-api, sourceapp-worker, sourceapp-webapp) when using grouped env vars · [Pre-Push Validation](PRE_PUSH_VALIDATION.md) — Run `npm run validate:production` before pushing backend changes; prevents wasted Render build time · [Render API Access Setup](RENDER_API_ACCESS_SETUP.md) — API key or CLI for programmatic access

**Examination:** [Screen Examination SOP](examination/SCREEN_EXAMINATION_SOP.md) · [Backend Examination SOP](examination/BACKEND_EXAMINATION_SOP.md) · [User Flow Identification SOP](examination/USER_FLOW_IDENTIFICATION_SOP.md)

**Coordination:** [Agent Procedure — Full Order](coordination/AGENT_PROCEDURE_ORDER.md) — **Required.** Single ordered checklist (Steps 0–14) for every work session; do all in order. · [Active Work SOP](coordination/ACTIVE_WORK_SOP.md) · [Repo Integrity Check SOP](coordination/REPO_INTEGRITY_CHECK_SOP.md) · [Implementation Verification SOP](coordination/IMPLEMENTATION_VERIFICATION_SOP.md) · [Documentation Agent SOP](coordination/DOCUMENTATION_AGENT_SOP.md) · [Production Deployment Validation SOP](coordination/PRODUCTION_DEPLOYMENT_VALIDATION_SOP.md) · [Manual Sync Trigger SOP](coordination/MANUAL_SYNC_TRIGGER_SOP.md) · [Commit Changes SOP](coordination/COMMIT_CHANGES_SOP.md) · [Operational Asset Creation and Reinforcement SOP](coordination/OPERATIONAL_ASSET_CREATION_AND_REINFORCEMENT_SOP.md) — Creating new SOPs/operational assets; make discoverable, reinforce in AI.md, commit per Commit Changes SOP

**Conventions:** [File Naming Conventions](conventions/FILE_NAMING_CONVENTIONS.md) · [Report Creation and Delivery SOP](conventions/REPORT_CREATION_AND_DELIVERY_SOP.md)

**Management:** [Integration Platforms Management SOP](management/INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md) · [Environment Variables Management SOP](management/ENVIRONMENT_VARIABLES_MANAGEMENT_SOP.md) · [Legal Documents SOP](management/LEGAL_DOCUMENTS_SOP.md) · [Content Documents SOP](management/CONTENT_DOCUMENTS_SOP.md)

**Usage (behavior docs):** [Authentication Flows](usage/AUTHENTICATION_FLOWS_USAGE_SOP.md) · [API Matrix](usage/API_MATRIX_USAGE_SOP.md) · [UI Integration Points](usage/UI_INTEGRATION_POINTS_USAGE_SOP.md) · [Design System](usage/DESIGN_SYSTEM_USAGE_SOP.md) · [Subscription Tiers](usage/SUBSCRIPTION_TIERS_USAGE_SOP.md) · [Intended User Flows](usage/INTENDED_USER_FLOWS_USAGE_SOP.md)

---

**Navigation:** Use `docs/README.md` Layer 6 as the spine; it links to every operation. This README is an index for the subfolder layout.
