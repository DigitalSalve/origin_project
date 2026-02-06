# UI Integration Points — Usage SOP

**Purpose:** Defines when and how to use `docs/04-behavior/UI_INTEGRATION_POINTS.md`, who uses it, and how to keep the route inventory and “intended” behavior in sync with the app.

**Status:** Active

**Applies To:** Frontend developers, product owners, QA, and AI agents working on routes, screens, or UI–backend wiring.

---

## Overview

This SOP ensures that:

1. **When to use** — Teams use the doc when adding/removing routes, defining “intended” behavior for a screen, or reconciling UI with backend (e.g. wiring readiness snapshot).
2. **Who uses it** — Frontend (route layout and intended API per screen), product (intended functions and scope), QA (what to test per route), migration/ops (coverage and gaps).
3. **How to keep it updated** — Update when routes change, when “intended” behavior or API touchpoints change, or when wiring readiness changes; keep the snapshot and section structure accurate.
4. **How it fits** — Layer 4 (behavior). It is the route-level complement to API_MATRIX (endpoint-level). Screen Examination and migration use it as the “intended” baseline.

---

## Prerequisites

Before using or updating `UI_INTEGRATION_POINTS.md`:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - `docs/04-behavior/UI_INTEGRATION_POINTS.md`
   - `docs/04-behavior/API_MATRIX.md` (endpoint ⇄ component map)
   - `docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md` if you are comparing “intended” to “actual” screens

2. **Understand structure:**
   - **Wiring Readiness Snapshot:** High-level % by area (auth, homeowner, contractor, admin, etc.); update when major areas change.
   - **Per-route sections:** Route path, “Intended” (function/purpose), “API” (touchpoints). Routes and role names are template/reference; for a specific app, use that app’s routes and roles.

3. **Note on template vs app:** When using this for a specific app, replace template routes and role names with the app’s actual routes and role structure.

---

## Process

### Step 1: Decide When to Open the Doc

- [ ] **Adding or removing a route:** Document the new route’s intended function and API touchpoints; remove or archive the old route.
- [ ] **Defining “intended” for a screen:** Use the doc to state what the route is for and which APIs it should use; avoid drift from product intent.
- [ ] **Assessing wiring readiness:** Use the snapshot and per-route “API” to see how much of the UI is wired; update percentages when large areas are completed or redesigned.
- [ ] **Running Screen Examination or migration:** Use this doc as the “intended” baseline to compare against actual screens and APIs.

**Output:** Clear purpose for opening the doc (implementation, definition, or assessment).

---

### Step 2: Use the Doc for Your Task

- [ ] **Find the route or area:** Use Global, Homeowner, Contractor, Admin (or this app’s equivalent) and the route path.
- [ ] **Read “Intended” and “API”:** Use them to implement or test; ensure new code aligns with “intended” and that “API” matches API_MATRIX and backend.
- [ ] **Use the snapshot for scope:** Wiring readiness percentages show relative completeness; use them to prioritize work and report status.

**Output:** Correct application of “intended” behavior and API touchpoints to build or test.

---

### Step 3: Update the Doc When Routes or Behavior Change

- [ ] **New route:** Add path, Intended, and API under the right area (Global/Homeowner/Contractor/Admin or app equivalent).
- [ ] **Changed “intended” or API:** Update the corresponding route block and adjust the snapshot if it affects a whole area.
- [ ] **Removed or deprecated route:** Remove or mark deprecated; document redirect or replacement if any.
- [ ] **Wiring readiness change:** After major work in an area, re-estimate and update the snapshot bullets and any area-level notes.

**Output:** Route inventory and “intended” behavior stay accurate and useful.

---

### Step 4: Keep Layer 4 Consistent

- [ ] **API_MATRIX.md:** Every “API” bullet here should have a matching endpoint ⇄ component entry in the matrix where applicable.
- [ ] **AUTHENTICATION_FLOWS.md:** Auth routes and “intended” auth behavior here must match the flows doc.
- [ ] **Screen Examination reports:** “Intended” in this doc is the baseline for “intended vs actual” in examination outputs; keep alignment so gap analysis is meaningful.

**Output:** No contradictions between UI Integration Points, API Matrix, and Authentication Flows; examinations stay comparable over time.

---

## Integration with Other Processes

- **API_MATRIX.md:** Provides the detailed endpoint ⇄ component map; this doc provides the route-level “intended” and API list. Keep them in sync.
- **AUTHENTICATION_FLOWS.md:** Auth routes and flows; any auth route in this doc should match the flows doc.
- **Screen Examination SOP:** Uses this doc as the source of “intended” behavior and API touchpoints when examining each screen.
- **Migration / Backend Examination:** Wiring readiness and route inventory inform scope and gap lists.

---

## Notes

- “Intended” is product/design intent; “API” is the set of backend touchpoints. Implementation may lag; the doc should still state intent so gaps are visible.
- When the app uses different routes or role names than the template, replace them with this app’s structure so the doc is the single source of truth for this project.
- Snapshot percentages are approximate; update when doing broad reviews (e.g. after a release or migration phase).

---

**Last Updated:** 2026-01-27
