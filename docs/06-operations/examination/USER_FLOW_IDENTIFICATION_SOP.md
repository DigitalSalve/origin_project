# User Flow Identification SOP

**Purpose:** Standard Operating Procedure for identifying intended user flows across the entire site or app map and documenting the full intended experience. Use this when you need a single, role-aware map of "what should happen" from first touch through each major journey, so design, implementation, and verification share one source of truth for the intended experience.

**Status:** Active

**Applies To:** Web applications, mobile applications, and other product experiences with multiple screens and roles. Use when documenting a new product, after major feature adds, or when aligning migration/verification to "intended experience."

---

## Overview

This SOP defines a systematic process to:

1. **Build or reuse a full app/site map** — Every route, screen, and layout; entry points and exit points.
2. **Identify roles and entry points** — Who uses the product, and where they can start (e.g. login, deeplink, public page).
3. **Enumerate intended flows by role and journey** — Auth, onboarding, core use cases, admin, and edge flows (logout, errors, 404).
4. **Document each flow** — Steps, screens, decisions, outcomes, and touchpoints (APIs, forms, external systems).
5. **Capture cross-cutting behavior** — Session, permissions, global navigation, and error/empty states that affect multiple flows.
6. **Produce an Intended User Flows document** — Authoritative description of the intended experience, linked from behavior docs and usable for QA, migration, and feature planning.

The output is an **Intended User Flows** (or **Intended Experience**) document in `docs/04-behavior/` plus an optional dated **User Flow Map Report** in `docs/08-status/user-flows/` when you run the process as an explicit audit.

---

## Prerequisites

Before beginning a user-flow identification run:

1. **Read relevant documentation:**
   - `AI.md` (repository-specific AI behavior rules)
   - `docs/README.md` (documentation navigation)
   - `docs/04-behavior/` (existing flows, UI integration points, auth, API matrix)
   - `docs/03-architecture/` (system map, routing, roles) when present
   - `docs/02-canon/` (roles, invariants) when present

2. **Obtain or create an app/site map:**
   - **From code:** Use routing config, file-based routes, or a Screen Examination Report.
   - **From design:** Use sitemaps, flow diagrams, or product specs.
   - **From both:** Merge so every route/screen that exists in the product is on the map.
   - **Scope:** Decide whether you are documenting "whole product" or a bounded area (e.g. "member app only," "admin only").

3. **Identify roles and access rules:**
   - List every user role or actor type (e.g. anonymous, member, admin, county_admin).
   - Note where role is determined (login, invite, segment) and where it gates screens or actions.
   - Use canon, auth flows, or permissions docs when they exist.

---

## Process

### Step 1: Build or Reuse the App/Site Map

Produce a single map of the entire surface users can reach.

#### 1.1 Enumerate All Routes / Screens

- [ ] **List every navigable route or screen** — From routing config, `app/` (or equivalent), or a Screen Examination inventory.
- [ ] **Include layouts only as structure** — Note tab groups, stacks, and shells so you know "this flow uses these N screens inside this shell."
- [ ] **Note entry and exit points** — First URL/screen(s) for each role; logout, session-expired, and external redirects.

**What to look for:**
- Routes that exist in code but are not reachable from any documented flow (orphans or future work).
- Screens that are "same route, different state" (e.g. empty vs. loaded) and whether you document them as one step or two.

**Output:** An **App/Site Map** table or outline, e.g.:

| Route / Screen | Role(s) | Shell / Layout | Entry? | Notes |
|----------------|---------|----------------|--------|-------|
| /login | All (unauthenticated) | Auth | Yes | Primary entry when not logged in |
| /onboarding | Member | Tabs | Conditional | If member and missing profile fields |
| /(tabs)/home | Member, Admin | Tabs | Yes (post-auth) | Home tab |
| … | … | … | … | … |

Use "Entry?" to mark primary or conditional entry points. Attach or reference the full inventory (e.g. Screen Examination Report or GOP_APP_INVENTORY_AND_PLAN_COVERAGE) so the map is traceable.

#### 1.2 Group by Area and Role

- [ ] **Group routes into areas** — e.g. Auth, Onboarding, Main app (tabs), Admin, Party directory, Standalone (donate, polling, etc.).
- [ ] **Per area, list allowed roles** — Which roles can reach which routes; note "redirect if wrong role" where applicable.

**Output:** Same map with Area and Role columns (or a short "Areas and role access" section). No route should be unassigned to an area.

---

### Step 2: Identify Roles and Entry Points

Make explicit who can use the product and where they start.

#### 2.1 List Roles and How They Are Determined

- [ ] **List every role/actor** — e.g. Anonymous, Member, Leader, County Admin, State Admin, Super Admin.
- [ ] **State how role is set** — Login, profile field, invite, segment, or config.
- [ ] **State how role can change** — Re-login, admin change, or "fixed after onboarding."

**Output:** A **Roles** table or list: Role name, How determined, Can change?, Notes.

#### 2.2 List Entry Points per Role

- [ ] **For each role, list every "first touch"** — Deep link, marketing link, open app, session restore.
- [ ] **For each entry, state the first screen** — e.g. "Open app → Index → (if no user) Login | (if user, incomplete profile) Onboarding | (else) Home."
- [ ] **Note conditional entries** — e.g. "Member with no address → Onboarding."

**Output:** An **Entry Points** section: Role → Entry type → First screen(s). Prefer a short decision tree or table.

---

### Step 3: Enumerate Intended Flows by Role and Journey

Identify every distinct "journey" you will document. A flow is a sequence of steps a user can take to achieve a goal, from a defined start to a defined end.

#### 3.1 Flow Catalog

- [ ] **Auth and session flows** — Login (password, OAuth, magic link), sign-up, logout, session restore, session expired, "remember this device."
- [ ] **Onboarding / profile-completion flows** — First-time profile, address, interests, role-specific onboarding.
- [ ] **Core member flows** — One flow per major use case (e.g. "View news," "Find events and RSVP," "Browse deals and redeem," "Donate," "Use party directory," "Look up polling location").
- [ ] **Admin flows** — One flow per admin capability (e.g. "Manage news," "Manage events," "Manage users/roles," "Send notifications").
- [ ] **Cross-role or edge flows** — Password reset, delete account, 404, maintenance, network error, empty states.

**What to look for:**
- Flows that span multiple areas (e.g. "Donate" might start from Home or Candidate and end on thank-you or payment provider).
- Alternate paths (e.g. "Login with Google" vs "Login with email") — document as one flow with branch steps, or as sub-flows, and be consistent.
- Flows that differ only by role (e.g. "Member views news" vs "Admin creates news") — separate flows when the steps or outcomes differ.

**Output:** A **Flow Catalog** — Named list of flows, each with: Flow name, Primary role(s), Start, End, and one-line goal. Example:

| Flow ID | Name | Role(s) | Start | End | Goal |
|---------|------|---------|-------|-----|------|
| F1 | Login | Unauthenticated | App open / Login link | Home or Onboarding | Authenticate and reach main app or onboarding |
| F2 | Onboarding | Member | Redirect when profile incomplete | Home | Complete profile and reach home |
| F3 | View and RSVP to event | Member | Home or Events tab | Event detail, then “You’re going” | View event and confirm attendance |
| … | … | … | … | … | … |

---

### Step 4: Document Each Flow (Steps, Screens, Decisions, Outcomes, Touchpoints)

For each flow in the catalog, write the intended experience in a fixed format so it can be used for implementation and verification.

#### 4.1 Flow Template (Use for Every Flow)

For each flow, produce a section that includes:

- **Flow ID and name**
- **Role(s)** that can run this flow
- **Preconditions** — e.g. "User is logged in," "User has completed onboarding."
- **Start** — Trigger or entry (e.g. "User taps Events tab," "User opens /event/123.")
- **Steps** — Numbered. Each step:
  - **Screen / route** — Where the user is.
  - **User action** — What they do (tap, type, submit, scroll).
  - **System response** — What the app does (navigate, call API, show message).
  - **Decisions** — If the path splits (e.g. "If no events → show empty state | else → show list").
- **End** — Success and, if relevant, failure end states (e.g. "User sees confirmation" or "User sees error, remains on form").
- **Touchpoints** — APIs, forms, or external systems (e.g. "POST /api/events/:id/rsvp," "Stripe checkout").

**Output:** One structured subsection per flow. Use a shared heading style, e.g. `### Flow F3: View and RSVP to event`.

#### 4.2 Consistency Rules

- [ ] **Screens are from the app/site map** — Use the same route/screen names as in Step 1.
- [ ] **Touchpoints match behavior docs** — Align with API Matrix, UI Integration Points, or Auth Flows when those exist; add or update them if you discover gaps.
- [ ] **Decisions are explicit** — Write "If X then step A, else step B" so QA and implementation can follow the same branches.
- [ ] **No implementation detail unless necessary** — Prefer "User submits form; system persists and shows confirmation" over "User clicks Save; we call POST /api/me."

**What to look for:**
- Flows that assume a screen or API that isn’t on the map — add to map or mark as "To be implemented."
- Missing error/empty branches — add a step or note: "If API error → show [specific] message and [retry or leave]."

---

### Step 5: Capture Cross-Cutting Behavior

Document behavior that affects many flows so it isn’t repeated in every flow and so verification can treat it once.

#### 5.1 Session and Auth

- [ ] **When session is required** — Which routes/screens require login; where unauthenticated users are sent.
- [ ] **When session is re-checked** — App startup, tab switch, before sensitive actions.
- [ ] **Session expiry and restoration** — What the user sees when session expires; whether "remember me" or similar exists.

**Output:** A short **Session and auth** subsection (or pointer to Auth Flows doc) that the flow doc can reference (e.g. "All flows assume session behavior per AUTHENTICATION_FLOWS.md except F1 Login and F0 Password reset").

#### 5.2 Permissions and Role Gates

- [ ] **Role-based routing** — Which roles can reach which areas; what happens when a wrong-role user hits a URL (redirect, 403, or 404).
- [ ] **Role-based actions** — Actions that only some roles can do (e.g. "Create news" for admins). Reference from the relevant flows.

**Output:** A **Permissions and role gates** subsection or table: Area or action → Allowed role(s) → Behavior when not allowed.

#### 5.3 Global Navigation and Shells

- [ ] **Tab order and labels** — For tabbed shells, list tabs in order and describe what each is for.
- [ ] **Persistent UI** — Header, drawer, bottom nav: what’s always there and how it affects flows (e.g. "Hamburger opens drawer; drawer has links to Profile, Logout, …").
- [ ] **Back and exit** — How "back" and "exit" work in each shell (e.g. stack back vs tab switch).

**Output:** A **Global navigation and shells** subsection so each flow can say "Uses Main Tabs" or "Uses Auth stack" without re-describing the shell.

#### 5.4 Errors and Empty States

- [ ] **Network/API errors** — Intended message and next action (retry, go back, contact support).
- [ ] **Empty lists / empty search** — Intended copy and any follow-up action (e.g. "No events — try different filters").
- [ ] **Validation errors** — Where they appear (inline, toast, modal) and that they block submit until resolved.

**Output:** An **Errors and empty states** subsection (or pointers to design/behavior docs) so flows can say "On error → see [Errors section]" where relevant.

---

### Step 6: Produce the Intended User Flows Document and Optional Report

Turn the flow catalog, flow write-ups, and cross-cutting sections into the deliverable(s).

#### 6.1 Authoritative Document in Behavior (Required)

- [ ] **Create or update** `docs/04-behavior/INTENDED_USER_FLOWS.md` (or project-chosen name, e.g. `INTENDED_EXPERIENCE.md`).
- [ ] **Structure:**  
  1. Brief purpose and scope (app name, “whole product” or bounded area).  
  2. Pointer to app/site map (or inline minimal map).  
  3. Roles and entry points (from Step 2).  
  4. Flow catalog (from Step 3).  
  5. One section per flow (from Step 4).  
  6. Cross-cutting behavior (from Step 5).  
  7. "Last updated" and link to this SOP.
- [ ] **Link from** `docs/README.md` Layer 4 (Behavior) so the doc is part of the spine.
- [ ] **Link from** related behavior docs (e.g. AUTHENTICATION_FLOWS, UI_INTEGRATION_POINTS) where they reference "intended flows" or "intended experience."

**Output:** `docs/04-behavior/INTENDED_USER_FLOWS.md` (or equivalent) as the single source of truth for the intended experience.

#### 6.2 Optional Dated Report (Audit / Runs)

When you run this process as an explicit audit (e.g. after a big release or for migration), you may also produce a dated report:

- [ ] **Create** `docs/08-status/user-flows/{YYYY-MM-DD}_USER_FLOW_MAP_REPORT.md`.
- [ ] **Content:** Summary (scope, roles, flow count), date, examiner, link to this SOP; high-level flow catalog and any "gaps vs. implementation" or "changes since last run"; link to the authoritative doc `docs/04-behavior/INTENDED_USER_FLOWS.md`.
- [ ] **Index:** Add link in `docs/08-status/user-flows/README.md` if present; ensure docs/README Layer 8 links to folder when notable.

**Output (optional):** Dated User Flow Map Report under `docs/08-status/user-flows/`.

---

## Output / Deliverable

**Primary deliverable:** **Intended User Flows** document.

**Location and naming:** `docs/04-behavior/INTENDED_USER_FLOWS.md` (or project-equivalent such as `INTENDED_EXPERIENCE.md`). Name and path may be set per project in behavior docs or README.

**When to create or update:** When defining or refining the intended experience for the whole app/site or a bounded area; after adding major features or roles; when preparing for migration or verification so "intended" is explicit.

**Optional deliverable:** **User Flow Map Report** — `docs/08-status/user-flows/{YYYY-MM-DD}_USER_FLOW_MAP_REPORT.md` for audit runs. Create when the process is run as a formal pass; link to the behavior doc and note scope and any gap findings.

---

## Report Template (Optional Dated Report)

Use this when producing a User Flow Map Report in `docs/08-status/user-flows/`:

```markdown
# User Flow Map Report

**Date:** YYYY-MM-DD
**Examiner:** [AI Agent / named examiner]
**Repository:** [Repository or app name]
**Scope:** [Full product / bounded area — e.g. "Member app and Admin; exclude partner portal"]
**Process:** Per `docs/06-operations/examination/USER_FLOW_IDENTIFICATION_SOP.md`

---

## 1. Summary

- **App/site map source:** [e.g. Screen Examination Report 2026-01-25, routing config]
- **Roles identified:** [number and names]
- **Flows documented:** [number]
- **Authoritative doc:** [link to docs/04-behavior/INTENDED_USER_FLOWS.md]

**Changes or gaps:** [Bullet list of "New flows added," "Flows removed," "Gaps vs. current implementation," or "No changes."]

---

## 2. Flow Catalog (Summary)

| Flow ID | Name | Role(s) | Start | End |
|---------|------|---------|-------|-----|
| … | … | … | … | … |

---

## 3. Links

- **Intended User Flows (source of truth):** [path]
- **App/site map or inventory:** [path]
- **Related:** [Auth flows, UI Integration Points, etc.]
```

---

## Integration with Other Processes

- **Screen Examination SOP:** The screen inventory and route list from a Screen Examination Report (or similar) are the preferred input for the app/site map (Step 1). Flows then attach to those screens; gaps between "screens that exist" and "screens used in flows" indicate missing or orphan screens.
- **Backend Examination SOP:** Touchpoints in each flow (APIs, webhooks) should align with the Backend Examination Report. Discrepancies ("flow calls X, backend has no X") become implementation or design gaps.
- **UI Integration Points / API Matrix:** INTENDED_USER_FLOWS describes journeys; UI Integration Points and API Matrix describe route-to-API wiring. Keep them consistent: flows reference routes and APIs that appear in those behavior docs, and vice versa.
- **Authentication Flows:** Auth and session behavior in INTENDED_USER_FLOWS should match or reference AUTHENTICATION_FLOWS.md. Prefer one authoritative auth description and have the flows doc point to it.
- **Migration Process:** In Discover/Document phases, running this SOP produces the "intended experience" against which the migrated product is verified. The Intended User Flows doc can be the reference for Phase 5 (Verify).
- **Documentation Process:** Treat INTENDED_USER_FLOWS as a behavior doc: update it when the intended experience changes, and note updates in change logs or feature notes as per DOCUMENTATION_PROCESS.

---

## Notes

- **Intended, not as-built:** This SOP documents the *intended* experience. For "as-built" or current implementation gaps, use Screen Examination and Backend Examination; compare their findings to this doc to get gap lists.
- **Role-first:** Organizing by role and entry point avoids missing flows that only exist for one segment (e.g. admins, first-time users).
- **One flow, one place:** Each flow is fully described in INTENDED_USER_FLOWS; other docs (feature specs, tickets) should point to that section rather than re-describe the flow.
- **Bounded scope is fine:** You can run this SOP for "Member app only" or "Auth and onboarding only" and state the scope clearly in the doc and report.

---

**Last Updated:** 2026-01-27
