# Authentication Flows — Usage SOP

**Purpose:** Defines when and how to use `docs/04-behavior/AUTHENTICATION_FLOWS.md`, who owns it, and how to keep it accurate as the source of truth for auth behavior.

**Status:** Active

**Applies To:** Frontend and backend developers, product owners, and AI agents implementing or changing authentication, session, or role-based access.

---

## Overview

This SOP ensures that:

1. **When to use** — Teams know when to read Authentication Flows (implementing or changing auth, debugging login/registration, wiring UI to auth endpoints).
2. **Who uses it** — Frontend (login/register UI, session handling), backend (magic-link and session APIs), product (flows and copy).
3. **How to keep it updated** — Add or edit sections when new flows, roles, or session behavior are introduced; keep routes, headers, and account-state rules in sync with code.
4. **How it fits** — Layer 4 (behavior). Canon and architecture define invariants and structure; this doc describes user-visible auth behavior and flows. Operations SOPs (e.g. Screen/Backend Examination) may reference it when checking auth screens or APIs.

---

## Prerequisites

Before using or updating `AUTHENTICATION_FLOWS.md`:

1. **Read relevant documentation:**
   - `AI.md` (repository behavior and documentation order)
   - `docs/README.md` (navigation and Layer 4 intent)
   - `docs/04-behavior/AUTHENTICATION_FLOWS.md` (the document this SOP governs)
   - Canon/architecture docs if the change touches session invariants or auth boundaries

2. **Understand scope:**
   - Auth flows live in Layer 4 (behavior), not in Layer 2 (canon) or Layer 3 (architecture).
   - The doc describes *what users and clients do* (routes, steps, backup codes, session behavior), not *how* tokens or DB tables are implemented (that belongs in architecture/code).

3. **Identify impact:**
   - New role or new entry route → add a flow section and update “When to Read” / related docs.
   - New header, cookie, or client type → update session-management and mobile sections.
   - Change in account states or bans → update “Account States” and any role tables.

---

## Process

### Step 1: Decide When to Open the Doc

- [ ] **Implementing or changing auth:** e.g. new magic-link flow, new role, new client type (web/mobile).
- [ ] **Debugging login/registration:** use the doc to confirm intended routes, steps, and backup code behavior.
- [ ] **Wiring UI to auth:** use route list and “Key Features” to ensure screens call the right endpoints and handle session/device headers.
- [ ] **Defining product behavior:** use it to align copy, step order, and expiration/rate-limit behavior.

**Output:** Decision that you are in scope for this doc; if not, use Layer 2/3 or ops SOPs instead.

---

### Step 2: Use the Doc for Your Task

- [ ] **Locate the right section:** Login Flows, Registration Flows, Session Management, Mobile Auth, Account States, or Design Patterns.
- [ ] **Check routes and endpoints:** ensure your code (or spec) matches the documented routes and API contract.
- [ ] **Check session/device rules:** cookies vs bearer token, device_id/client_type, idle/absolute timeouts, trusted device.
- [ ] **Preserve structure:** if adding a new flow, follow the same format (Route, Flow, Features, links to related docs).

**Output:** Correct application of documented behavior to implementation or verification.

---

### Step 3: Update the Doc When Behavior Changes

- [ ] **New flow or role:** Add a numbered flow with Route, Flow steps, and Features; update Status Summary table and “When to Read.”
- [ ] **New session/device rules:** Update “Session Management” and “Device and Client Scoping”; if mobile, update “Mobile App Authentication” and endpoints list.
- [ ] **New account state or policy:** Update “Account States (enforced)” and any role matrix.
- [ ] **Route or endpoint renames:** Update every occurrence in the doc and ensure API_MATRIX / UI_INTEGRATION_POINTS stay in sync.

**Output:** `AUTHENTICATION_FLOWS.md` remains the single source of truth for auth behavior.

---

### Step 4: Keep Cross-References Accurate

- [ ] **Error handling:** Point to `docs/06-operations/ERROR_HANDLING_AND_RESILIENCE.md` (or equivalent) where relevant.
- [ ] **Feature/implementation docs:** Link to any `docs/08-status/feature-requests/` or architecture docs that define *how* a flow is implemented.
- [ ] **API Matrix / UI Integration Points:** When routes or endpoints change here, update `API_MATRIX.md` and `UI_INTEGRATION_POINTS.md` so Layer 4 stays consistent.

**Output:** No broken or stale links; behavior, API, and UI docs align.

---

## Integration with Other Processes

- **API_MATRIX.md:** Auth endpoints and UI components are listed there; auth flow changes here should be reflected in the matrix.
- **UI_INTEGRATION_POINTS.md:** Route inventory and “intended” behavior per screen; auth routes and “intended” auth behavior must match this doc.
- **Canon / Architecture:** Session invariants, banned-state rules, and storage decisions stay in Layer 2/3; this doc describes observable behavior only.
- **Screen / Backend Examination SOPs:** When examining auth screens or auth APIs, use this doc as the behavior baseline for “intended” vs “actual.”

---

## Notes

- The doc is product-agnostic (no product name in titles/copy). Use `[Product]` or the app name in templates (e.g. email “Sign in to [Product]”) when applying to a specific project.
- Do not move canonical rules (e.g. “super admin cannot be suspended”) into this doc; those stay in canon. This doc only describes how users and clients experience auth and sessions.
- When in doubt, treat “what the user sees and does” as behavior (this doc) and “what the system enforces” as canon/architecture.

---

**Last Updated:** 2026-01-27
