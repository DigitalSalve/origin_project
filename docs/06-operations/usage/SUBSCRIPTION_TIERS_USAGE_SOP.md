# Subscription Tiers — Usage SOP

**Purpose:** Defines when and how to use `docs/04-behavior/SUBSCRIPTION_TIERS.md`, who uses it, and how to keep tiers, entitlements, and flow intersections accurate.

**Status:** Active

**Applies To:** Product owners, backend and frontend developers, and AI agents implementing or changing subscription plans, billing, or plan-gated features.

---

## Overview

This SOP ensures that:

1. **When to use** — Teams use the doc when defining or changing plans, implementing plan-based gating (backend or UI), or tracing how subscriptions affect payments, email, branding, or custom code.
2. **Who uses it** — Product (tier definitions and entitlements), backend (enforcement and source-of-truth APIs), frontend (billing UI, plan-based feature visibility), migration/ops (gap analysis for “required work”).
3. **How to keep it updated** — Update when tiers or entitlements change, when implementation status changes, or when new “flow intersections” (payments, email, branding, custom code) are added or resolved.
4. **How it fits** — Layer 4 (behavior). It describes *what* each tier allows and how plans intersect other flows; it does not define billing system architecture (Layer 3) or external payment contracts (Layer 5).

---

## Prerequisites

Before using or updating `SUBSCRIPTION_TIERS.md`:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - `docs/04-behavior/SUBSCRIPTION_TIERS.md`
   - Canon/architecture if subscription or billing is constrained by invariants or system boundaries
   - `docs/04-behavior/DESIGN_SYSTEM.md` if plan-based branding/theming is involved

2. **Understand structure:**
   - **Source of Truth:** APIs and stores for pricing and subscription state.
   - **Tiers and Entitlements:** What each plan includes (e.g. Free, Professional, Enterprise).
   - **Implementation Status:** Implemented vs not yet implemented.
   - **Flow Intersections:** How plans affect Payments, Email, Contractor Portal Branding, Custom Code.
   - **Required Work:** Ordered list of changes to enforce or complete plan behavior.

3. **Note on branding:** The doc uses product-agnostic terms (e.g. “platform-branded,” “reduced platform branding”). For a specific app, substitute the app name where useful, but keep the structure reusable.

---

## Process

### Step 1: Decide When to Open the Doc

- [ ] **Defining or changing a tier:** Document new/updated plan name, entitlements, and limits.
- [ ] **Implementing plan gating:** Use “Tiers and Entitlements” and “Required Work” to decide what to build next; use “Implementation Status” to avoid duplicating work.
- [ ] **Tracing impact of plans:** Use “Flow Intersections” to see how subscriptions affect payments, email, branding, custom code.
- [ ] **Reporting gaps:** Use “Not Implemented Yet” and “Required Work” in status or migration docs; keep this doc updated when gaps are closed.

**Output:** Clear use case (define, implement, trace, or report).

---

### Step 2: Use the Doc for Your Task

- [ ] **Locate the right section:** Source of Truth, Tiers and Entitlements, Implementation Status, Flow Intersections, or Required Work.
- [ ] **Respect “Implemented” vs “Not Implemented”:** Do not describe or depend on behavior as if it exists unless it is listed as implemented; use “Required Work” as the backlog.
- [ ] **Check flow intersections:** Before changing payments, email, branding, or custom code, see how the doc says plans should affect them; implement or document deviation.

**Output:** Correct application of tier behavior and implementation status to design or code.

---

### Step 3: Update the Doc When Tiers or Implementation Change

- [ ] **New or changed tier:** Update “Tiers and Entitlements”; add or adjust bullets for limits, branding, and features.
- [ ] **New entitlement or limit:** Add to the right plan and, if it affects a flow, add or update “Flow Intersections.”
- [ ] **Implementation completed:** Move items from “Not Implemented Yet” to “Implemented” and add a brief note; trim or reorder “Required Work” when items are done.
- [ ] **New flow intersection:** Add a subsection under “Flow Intersections” (e.g. “Integrations” or “Reporting”); describe current behavior and target behavior by plan.
- [ ] **Source of truth change:** Update “Source of Truth” (APIs, tables) so the doc matches where pricing and subscription state actually live.

**Output:** Subscription Tiers remains the single source of truth for plan behavior and implementation status.

---

### Step 4: Keep Cross-References and Status Accurate

- [ ] **DESIGN_SYSTEM.md:** Plan-based branding (e.g. “reduced platform branding”) should be implementable using the design system; keep the two docs aligned when theming or branding is plan-driven.
- [ ] **AUTHENTICATION_FLOWS.md:** No direct tie unless plan affects auth (e.g. plan-gated SSO); if so, add a flow intersection and reference the flows doc.
- [ ] **CURRENT_STATUS / Migration / Feature requests:** When “Required Work” items are done or reprioritized, update this doc and any status or feature-request docs that reference it.
- [ ] **Canon:** If subscription or billing rules are mandated in Layer 2, this doc should not contradict them; document “per canon” where relevant.

**Output:** No stale “Required Work” or “Not Implemented” items; behavior and status docs stay in sync.

---

## Integration with Other Processes

- **DESIGN_SYSTEM.md:** Plan-based branding and theming; design system defines how to implement “platform-branded” vs “reduced platform branding” and custom colors/logos.
- **API_MATRIX / UI_INTEGRATION_POINTS:** Billing and subscription screens and endpoints should appear there; tier-related visibility and gating are *behavior* described here.
- **Feature requests / Implementation Verification:** “Required Work” and “Not Implemented Yet” should be reflected in feature requests and verification docs when gaps are tracked or closed.
- **Backend Examination:** Enforcement of plan-based entitlements (e.g. custom code, custom domain) can be examined against this doc’s “Implementation Status” and “Required Work.”

---

## Notes

- The doc is product-agnostic; “platform-branded” and “reduced platform branding” apply to whichever product uses this base. Do not reintroduce product-specific names unless the project explicitly adopts them.
- “Required Work” is a living list; reorder and reprioritize as the project advances, and move items out when done.
- Do not add tiers or entitlements that are not approved (e.g. by product or canon); treat this doc as the agreed behavior, not a wish list.

---

**Last Updated:** 2026-01-27
