# Intended User Flows — Usage SOP

**Purpose:** Defines when and how to use `docs/04-behavior/INTENDED_USER_FLOWS.md` (or project-chosen equivalent such as `INTENDED_EXPERIENCE.md`), who owns it, and how to keep it accurate as the source of truth for the intended experience. **Creation** of the doc is governed by [User Flow Identification SOP](../examination/USER_FLOW_IDENTIFICATION_SOP.md); this SOP governs **usage and maintenance** once the doc exists.

**Status:** Active

**Applies To:** Frontend and backend developers, product owners, QA, migration leads, and AI agents implementing flows, verifying "intended vs. actual," or planning features. Use when the Intended User Flows document exists in `docs/04-behavior/`.

---

## Overview

This SOP ensures that:

1. **When to use** — Teams know when to read Intended User Flows (implementing or changing a flow, QA planning, migration verification, feature planning, aligning "intended" with actual behavior).
2. **Who uses it** — Implementation (screen order, decisions, APIs per flow), QA (expected paths and outcomes), product (journey and copy), migration (intended experience to replicate).
3. **How to keep it updated** — When the intended experience changes (new flow, new role, new screen, changed step order), update the doc per the structure created by the User Flow Identification SOP; keep roles, entry points, and flow catalog in sync with product decisions.
4. **How it fits** — Layer 4 (behavior). Canon and architecture define invariants and structure; this doc describes the full intended experience by role and journey. UI Integration Points and API Matrix describe route-to-API wiring; Intended User Flows describe journeys that use those routes and APIs.

---

## Prerequisites

Before using or updating the Intended User Flows document:

1. **Confirm the doc exists:** `docs/04-behavior/INTENDED_USER_FLOWS.md` (or project-chosen path/name). If it does not exist, create it per [User Flow Identification SOP](../examination/USER_FLOW_IDENTIFICATION_SOP.md); this Usage SOP applies once it exists.

2. **Read relevant documentation:**
   - `AI.md` and `docs/README.md` (documentation order and Layer 4 intent)
   - `docs/04-behavior/INTENDED_USER_FLOWS.md` (the document this SOP governs)
   - `docs/04-behavior/UI_INTEGRATION_POINTS.md` and `docs/04-behavior/API_MATRIX.md` (routes and API wiring used by flows)
   - `docs/04-behavior/AUTHENTICATION_FLOWS.md` when the change touches auth or session

3. **Understand scope:**
   - The doc describes *what should happen* from first touch through each major journey (role, entry points, steps, screens, decisions, outcomes).
   - It does not describe implementation details (those belong in architecture/code); it describes the intended user-visible experience.

---

## Process

### Step 1: Decide When to Open the Doc

- [ ] **Implementing or changing a flow:** New screen, new step order, new role path, or changed decision/outcome in an existing flow.
- [ ] **QA or verification:** Use the doc as the baseline for "intended" when comparing to actual behavior (e.g. Screen Examination, migration verification).
- [ ] **Feature planning:** Use flow catalog and role/entry-point list to scope which flows are in or out of a feature.
- [ ] **Migration:** Use as the reference for "intended experience" to replicate or verify in the target stack.
- [ ] **Aligning with product:** Use it to align copy, step order, and error/empty states with product decisions.

**Output:** Decision that you are in scope for this doc; if the doc does not exist, use the User Flow Identification SOP to create it first.

---

### Step 2: Use the Doc for Your Task

- [ ] **Locate the right section:** App/site map, roles and entry points, flow catalog, or a specific flow (auth, onboarding, core use case, admin, edge flows).
- [ ] **Check role and entry points:** Ensure your implementation or verification matches the documented roles and "first touch" per role.
- [ ] **Check step order and decisions:** For the flow you are working on, ensure screens, decisions, and outcomes match the doc.
- [ ] **Check cross-cutting behavior:** Session, permissions, global navigation, error/empty states that affect multiple flows.
- [ ] **Preserve structure:** If adding a new flow, follow the same format used in the doc (e.g. Flow name, Role(s), Steps, Screens, Decisions, Outcomes, touchpoints).

**Output:** Correct application of documented intended experience to implementation or verification.

---

### Step 3: Update the Doc When the Intended Experience Changes

- [ ] **New flow or role:** Add a flow section with steps, screens, decisions, outcomes; update the flow catalog and roles/entry-points sections.
- [ ] **New or removed screen in a flow:** Update the relevant flow’s steps and screen list; ensure entry/exit points are still accurate.
- [ ] **Changed step order or decision:** Edit the flow section so it matches the product decision; avoid leaving the doc out of date.
- [ ] **New cross-cutting behavior:** Update the section on session, permissions, global nav, or error/empty states so all affected flows stay consistent.
- [ ] **Route or API renames:** Update flow steps and touchpoints so they reference current routes and APIs; keep INTENDED_USER_FLOWS consistent with UI_INTEGRATION_POINTS and API_MATRIX.

**Output:** `INTENDED_USER_FLOWS.md` (or equivalent) remains the single source of truth for the intended experience.

---

### Step 4: Keep Cross-References Accurate

- [ ] **UI Integration Points / API Matrix:** Flows reference routes and APIs that appear in those behavior docs. When you change a flow here, ensure UI_INTEGRATION_POINTS and API_MATRIX stay consistent (and vice versa).
- [ ] **Authentication Flows:** Auth and session behavior in Intended User Flows should match or reference AUTHENTICATION_FLOWS.md. Prefer one authoritative auth description and have the flows doc point to it.
- [ ] **User Flow Identification SOP:** When you do a major update (e.g. new area, new role set), consider running the User Flow Identification process as a formal pass and optionally producing a dated report in `docs/08-status/user-flows/`.
- [ ] **Feature requests / migration:** Link from feature-request or migration docs to the specific flow section when a feature or phase affects that flow.

**Output:** No broken or stale links; intended flows, routes, and API wiring stay aligned.

---

## Integration with Other Processes

- **User Flow Identification SOP:** Creates and structurally defines the Intended User Flows document. Use it when the doc does not exist or when doing a full audit pass; use this Usage SOP for day-to-day use and maintenance.
- **UI Integration Points / API Matrix:** Describe route inventory and endpoint ⇄ UI mapping; Intended User Flows describe journeys that use those routes and APIs. Keep them consistent.
- **Authentication Flows:** Auth and session in Intended User Flows should match or reference AUTHENTICATION_FLOWS.md.
- **Screen / Backend Examination:** When examining screens or APIs, use Intended User Flows as the "intended" baseline for comparison.
- **Migration Process:** The Intended User Flows doc is the reference for "intended experience" in Discover/Document/Verify phases.

---

## Notes

- **One flow, one place:** Each flow is fully described in INTENDED_USER_FLOWS; other docs (feature specs, tickets) should point to that section rather than re-describe the flow.
- **Creation vs. usage:** Creating the doc or doing a full audit is covered by the User Flow Identification SOP; this SOP covers when and how to use and maintain it.
- **Project-specific naming:** The authoritative doc may be named `INTENDED_USER_FLOWS.md`, `INTENDED_EXPERIENCE.md`, or similar; path is typically `docs/04-behavior/`. When the project uses a different path/name, note it in this SOP’s "Prerequisites" or in docs/README Layer 4.

---

**Last Updated:** 2026-01-27
