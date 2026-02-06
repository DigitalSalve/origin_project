# API Matrix — Usage SOP

**Purpose:** Defines when and how to use `docs/04-behavior/API_MATRIX.md`, who maintains it, and how to keep the endpoint ⇄ UI mapping accurate.

**Status:** Active

**Applies To:** Frontend and backend developers, integrators, and AI agents wiring UI to APIs or adding/removing endpoints.

---

## Overview

This SOP ensures that:

1. **When to use** — Teams use the matrix when wiring screens to endpoints, adding or changing APIs, or verifying that UI has the right backend contracts.
2. **Who uses it** — Frontend (which endpoints each screen/component calls), backend (which UI depends on each endpoint), product (critical vs secondary flows).
3. **How to keep it updated** — Update the matrix whenever an endpoint or a UI component is added, renamed, or removed; keep domain groupings and Critical/Second tag consistent.
4. **How it fits** — Layer 4 (behavior). It bridges “what the UI does” and “what the API offers.” Architecture describes structure; this doc describes the wiring map. Backend Examination and migration work use it to find gaps.

---

## Prerequisites

Before using or updating `API_MATRIX.md`:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - `docs/04-behavior/API_MATRIX.md`
   - `docs/04-behavior/UI_INTEGRATION_POINTS.md` (route inventory and “intended” behavior)
   - Canon/architecture if the change affects public API contracts or system boundaries

2. **Understand conventions:**
   - **Critical:** required for core money-movement or auth orchestration.
   - **Secondary:** supporting, admin, or non-blocking.
   - Endpoints are grouped by domain; UI refs use component paths (or equivalent for this app).

3. **Note on paths and roles:** Routes and component paths in the doc are template/reference. For a specific app, replace them with that app’s actual routes and component paths.

---

## Process

### Step 1: Decide When to Open the Doc

- [ ] **Wiring a screen to an API:** Check which endpoints the screen is supposed to call and add/confirm the mapping.
- [ ] **Adding or changing an endpoint:** Ensure every UI touchpoint is listed under that endpoint (and add new ones).
- [ ] **Removing or deprecating an endpoint:** Find all UI references in the matrix and update or remove them; track in feature-request/status if migration is needed.
- [ ] **Auditing “who uses this API”:** Use the matrix as the source of truth for UI dependents.

**Output:** Clear need to read or edit the matrix.

---

### Step 2: Use the Matrix for Your Task

- [ ] **Locate the domain:** Auth, Payment Sessions, Homeowner, Contractor, Admin, etc.
- [ ] **Find endpoint or component:** Use endpoint path or component path; ensure you’re in the right app (template vs actual).
- [ ] **Confirm Critical vs Secondary:** Use the tag to prioritize; avoid marking non-core flows as Critical.
- [ ] **Check mobile/special clients:** Mobile and other client-specific endpoints are called out; use those sections when working on non-web UI.

**Output:** Correct understanding of UI ⇄ API wiring for your change.

---

### Step 3: Update the Matrix When Wiring Changes

- [ ] **New endpoint:** Add under the right domain; list every screen/component that calls it; set Critical or Secondary.
- [ ] **New screen/component:** Add it under every endpoint it calls; if it introduces a new endpoint, add that too.
- [ ] **Rename or remove:** Update all rows that reference the old path or component; keep groupings consistent.
- [ ] **New domain:** Add a new numbered section and follow the same format (endpoint ⇒ list of UI touchpoints).

**Output:** Matrix stays the single source of truth for “which UI uses which API.”

---

### Step 4: Keep Layer 4 Consistent

- [ ] **AUTHENTICATION_FLOWS.md:** If auth routes or endpoints change, update both the flows doc and the matrix.
- [ ] **UI_INTEGRATION_POINTS.md:** Route inventory and “intended” behavior should align with the matrix; when you add/remove wiring here, ensure the route inventory and “API” bullets there are updated.

**Output:** No contradictions between API Matrix, UI Integration Points, and Authentication Flows.

---

## Integration with Other Processes

- **UI_INTEGRATION_POINTS.md:** Per-route “intended” behavior and API touchpoints; the matrix is the detailed endpoint ⇄ component map for those touchpoints.
- **AUTHENTICATION_FLOWS.md:** Auth endpoints and flows; changes to auth wiring should be reflected in both docs.
- **Backend Examination SOP:** Examination outputs can reference the matrix to list “documented UI consumers” for each endpoint and compare to actual implementation.
- **Canon / Architecture:** Public API surface and backward-compatibility rules live in canon; the matrix describes current wiring, not policy.

---

## Notes

- Do not add endpoints that are not yet implemented without a clear “Planned” or “Not Yet Implemented” marker and a link to the feature request or design.
- When the app uses different routes or component paths than the template, the matrix should be updated to this app’s paths so it remains useful for this project.
- Critical vs Secondary drives prioritization in migration and outage impact; use consistently.

---

**Last Updated:** 2026-01-27
