# Documentation List for AI Projects

**Purpose:** This document lists the essential files to provide to a ChatGPT project (custom GPT, Claude project, etc.) so it can understand a project that uses this documentation structure in depth.

**We keep the AI instructions and general structure.** When you apply this base to a new project, you fill in project-specific content in layers 2–5; this list describes the full structure so you know what to create and in what order to provide it to an AI.

---

## Quick Reference: Essential Documents (Minimum Set)

If you can only provide a limited set, start with these **core documents**:

1. `AI.md` — AI operational guide and behavior rules
2. `docs/README.md` — Navigation spine to all documentation
3. `docs/01-orientation/PROJECT_IDENTITY.md` — **Source of truth** for project name, slug, repository role, and infrastructure vs project-specific boundary
4. `docs/02-canon/GUIDELINES.md` — AI behavior rules and documentation expectations
5. `docs/02-canon/SYSTEM_CANON.md` — Authoritative system truth (fill per project; create when applying base)
6. `docs/03-architecture/SYSTEM_MAP.md` — Quick index of how system is wired (fill per project)
7. `docs/04-behavior/AUTHENTICATION_FLOWS.md` — Authentication implementation (fill per project)
8. `docs/08-status/CURRENT_STATUS.md` — Current implementation status
9. `docs/08-status/feature-requests/FEATURE_REQUEST_TODO.md` — Prioritized feature list
10. `docs/06-operations/processes/DOCUMENTATION_PROCESS.md` — Documentation process and feature request lifecycle
11. `docs/01-orientation/GETTING_STARTED.md` — Setup and first steps

---

## Complete Documentation Set (Recommended)

For comprehensive understanding, provide documents in this order. Items marked "fill per project" are part of the **general structure**; create and fill them when applying this base to a new project.

### Layer 0: Entry Points
- `AI.md` — AI operational guide, behavior rules, required reading order
- `docs/README.md` — Navigation spine to all documentation layers

### Layer 1: Orientation
- `docs/01-orientation/PROJECT_IDENTITY.md` — Project name, slug, repository role, infrastructure vs project-specific
- `docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md` — Take this infrastructure to a new project without wiping; copy-this-repo vs template-repo workflows, explicit path list
- `docs/01-orientation/GETTING_STARTED.md` — Installation, setup, first steps

### Layer 2: Canon and Mandates (fill per project)
- `docs/02-canon/GUIDELINES.md` — AI behavior rules and documentation expectations
- `docs/02-canon/SYSTEM_CANON.md` — **Authoritative system truth** (how the system works end-to-end)
- `docs/02-canon/AUDIT_SCOPE.md` — Full codebase audit checklist (or project equivalent)
- `docs/02-canon/CONSENT_AND_AUDIT.md` — Consent records and audit logging (or project equivalent)

### Layer 3: Architecture (fill per project)
- `docs/03-architecture/SYSTEM_MAP.md` — Quick index of how system is wired
- `docs/03-architecture/DATABASE_RELATIONS_AND_USER_FLOWS.md` — Database schema and entity relationships (or project equivalent)
- `docs/03-architecture/STATE_MACHINE.md` — Core state transitions (if applicable)
- `docs/03-architecture/FUNDS_FLOW.md` or value-flow equivalent (if applicable)
- `docs/03-architecture/CONTEXT_ROUTING_SPEC.md` or routing/queuing spec (if applicable)
- `docs/03-architecture/POLICY_LIMITS.md` or policy enforcement (if applicable)
- `docs/03-architecture/USER_STATUS_MODEL.md` or user-account state (if applicable)
- `docs/03-architecture/ADMIN_ACCESS_AND_SUPPORT.md` or admin auth/impersonation (if applicable)

### Layer 4: Behavior (fill per project)
- `docs/04-behavior/AUTHENTICATION_FLOWS.md` — Authentication implementation (sessions, tokens, etc.)
- `docs/04-behavior/API_MATRIX.md` — Endpoint-to-screen integration map
- `docs/04-behavior/UI_INTEGRATION_POINTS.md` — Full route inventory with intended functions
- `docs/04-behavior/SUBSCRIPTION_TIERS.md` or tier entitlements (if applicable)
- `docs/04-behavior/DESIGN_SYSTEM.md` — UI/UX guidelines, color system, typography, components

### Layer 5: Integrations (fill per project)
- `docs/05-integrations/INTEGRATION_REQUIREMENTS.md` — Backend implementation and API wiring
- Provider-specific implementation docs (per external service)
- `docs/05-integrations/WEBHOOK_CONTRACTS.md` — Provider webhook verification and payload expectations
- Subfolders per provider for API references (as needed)

### Layer 6: Operations
- `docs/06-operations/processes/DOCUMENTATION_PROCESS.md` — **How to keep docs in sync with code, feature request lifecycle**
- `docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md` — Screen examination process
- Fill per project: deployment doc, error handling/resilience, archival lifecycle, legal/content (as needed)

### Layer 7: Guides
- `docs/07-guides/templates/FEATURE_NOTE.md` — Template for documenting completed features
- Completed feature notes (fill per project as features ship)

### Layer 8: Status
- `docs/08-status/CURRENT_STATUS.md` — Latest development updates
- `docs/08-status/feature-requests/FEATURE_REQUEST_TODO.md` — Prioritized list of feature requests
- `docs/08-status/IMPLEMENTATION_VERIFICATION.md` — Verified implementation status questions
- `docs/08-status/ACTIVE_WORK.md` — Currently checked-out files (coordination)
- `docs/08-status/ACTIVE_WORK_LOG.md` — Append-only check-out/check-in history
- `docs/08-status/qa/` — QA TODO, work log, completed reports (as needed)
- `docs/08-status/reports/` — Report log and guidelines

---

## By Use Case

### For Understanding Current Implementation
**Priority order:**
1. `docs/02-canon/SYSTEM_CANON.md` — System truth
2. `docs/03-architecture/DATABASE_RELATIONS_AND_USER_FLOWS.md` (or equivalent) — Data model
3. `docs/03-architecture/SYSTEM_MAP.md` — Code locations
4. `docs/08-status/CURRENT_STATUS.md` — What’s implemented
5. `docs/04-behavior/AUTHENTICATION_FLOWS.md` — Auth implementation
6. `docs/04-behavior/API_MATRIX.md` — API endpoints

### For Making Code Changes
**Priority order:**
1. `AI.md` — Behavior rules
2. `docs/02-canon/SYSTEM_CANON.md` — System truth (don’t violate)
3. `docs/03-architecture/SYSTEM_MAP.md` — Where code lives
4. `docs/06-operations/processes/DOCUMENTATION_PROCESS.md` — Process to follow
5. `docs/08-status/feature-requests/FEATURE_REQUEST_TODO.md` — What’s planned
6. Relevant architecture/behavior docs for the area you’re changing

### For Understanding Integrations
**Priority order:**
1. `docs/05-integrations/INTEGRATION_REQUIREMENTS.md` — Integration guide
2. Provider-specific implementation docs
3. `docs/05-integrations/WEBHOOK_CONTRACTS.md` — Webhook contracts

---

## Notes for AI Project Setup

### Recommended approach
1. **Start with the core documents** listed in "Quick Reference" above
2. **Add project-filled docs** in the order defined in `AI.md` (layers 1–8)
3. **Always include `AI.md`** — it contains critical behavior rules
4. **Always include `docs/README.md`** — it’s the navigation spine

### File organization tips
- **Group by layer** when uploading to an AI project
- **Use descriptive names** that match the layer structure
- **Include the full path** in file descriptions for reference

### Critical rules to emphasize (from AI.md)
1. **Feature Request Creation Process** — No implementation without a feature request
2. **Documentation Process Enforcement** — Complete documentation checklist before marking features complete
3. **Separation of Concerns** — Authority and decisions live where the project defines them
4. **Canon is Truth** — Layer 2 documents are mandates, not suggestions
5. **Read in Order** — Layers 1–8 are ordered for a reason

---

## Keeping This List Updated

Update this list when:
- New critical documentation is added to the base
- Documentation structure changes
- New layers or major sections are added

**Last Reviewed:** (set per project)
