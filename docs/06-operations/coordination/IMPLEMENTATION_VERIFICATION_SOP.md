# Implementation Verification — SOP

**Purpose:** Standard Operating Procedure for verifying implementation status, tracking open questions that need code review, and ensuring gaps are recorded in Project feature-request items. Governs when and how to use `docs/08-status/IMPLEMENTATION_VERIFICATION.md` and how verification ties to feature requests.

**Status:** Active

**Applies To:** AI agents and developers performing implementation verification, structural changes, or documentation verification who need to confirm whether documented/planned behavior exists in code and to track gaps.

---

## Overview

This SOP ensures that:

1. **When to run** — Verification is performed when checking implementation status, during structural changes, or when documentation verification raises questions that require code review to answer.
2. **Who runs it** — Anyone verifying that features are implemented as documented (agents, developers, QA) or making structural changes that could expose gaps.
3. **Single tracking location** — Open questions and resolved questions live in `docs/08-status/IMPLEMENTATION_VERIFICATION.md`; format and lifecycle are defined here.
4. **Gap tracking** — Implementation gaps identified during verification must be listed in the "Not Yet Implemented" (or equivalent) section of the relevant Project feature-request item (label `coordination:feature-request`). Never leave gaps untracked.
5. **Repeatable process** — Steps for adding questions, verifying (code/migration/test review), resolving, and updating Project feature-request items are consistent and discoverable via Layer 6.

---

## File Structure and Output Location

| File | Purpose | Governed By |
|------|---------|-------------|
| `docs/08-status/IMPLEMENTATION_VERIFICATION.md` | Open questions, resolved questions, verification session context | This SOP |

**Tables in IMPLEMENTATION_VERIFICATION.md:**
- **Open Questions** — Question, Source, Status, Assigned, Notes
- **Resolved Questions** — Question, Source, Resolved (date), Resolution

---

## Prerequisites

Before running implementation verification:

1. **Read:** `AI.md` "When finding implementation gaps," this SOP, and the current `docs/08-status/IMPLEMENTATION_VERIFICATION.md`.
2. **Identify scope:** Are you verifying a specific feature, doing a structural change that might expose gaps, or following up on documentation verification?
3. **Locate feature-request items:** Gaps must be tracked in the relevant Project item(s) (label `coordination:feature-request`).

---

## Process

### Step 1: Identify Questions or Gaps

- [ ] **Source of question:** Documentation verification, structural change, code review, or explicit verification pass.
- [ ] **Phrase clearly:** One row per question (e.g. "Is magic-link expiry enforced in backend?", "Does role X have access to route Y?").
- [ ] **Add to Open Questions** in `IMPLEMENTATION_VERIFICATION.md` with:
  - **Question** — Clear, answerable statement
  - **Source** — Where it came from (e.g. "Backend Examination 2026-01-27", "Feature request FLUTTER_PHASE1")
  - **Status** — Open, In progress, or similar
  - **Assigned** — Who is investigating (optional)
  - **Notes** — Context or partial findings

**Output:** New row(s) in Open Questions. If the question implies a **gap** (documented/planned but not implemented), proceed to Step 2 in parallel.

---

### Step 2: Ensure Gaps Are Tracked in Project Feature-Request Items

- [ ] **If the question reflects an implementation gap** — Ensure the gap is listed in the "Not Yet Implemented" (or equivalent) section of the relevant Project item (label `coordination:feature-request`).
- [ ] **If no feature-request exists** — Create one per [Documentation Process](../processes/DOCUMENTATION_PROCESS.md) (Project item with `coordination:feature-request`), then add the gap to that item body.
- [ ] **Ask:** "Is this gap tracked in the feature-request item?" If not, add it before closing the verification item.

**Output:** Every identified gap is reflected in a Project feature-request item. Never leave gaps untracked.

---

### Step 3: Verify via Code, Migration, and Test Review

- [ ] **Code review** — Check actual implementation in the codebase for the area in question.
- [ ] **Migration review** — If the question touches schema or data, check database migrations for consistency with docs.
- [ ] **Test review** — Check for tests covering the feature or behavior; note coverage gaps in Notes or in the Project feature-request item.
- [ ] **Update IMPLEMENTATION_VERIFICATION.md** — Add findings to Notes; update Status and Assigned as needed.

**Output:** Evidence-based answer to the question, recorded in Notes or in the next step.

---

### Step 4: Resolve and Record

- [ ] **When the question is answered:**
  - Move the row from **Open Questions** to **Resolved Questions** in `IMPLEMENTATION_VERIFICATION.md`.
  - Fill **Resolved** (date) and **Resolution** (short summary of what was found or done).
- [ ] **If the answer reveals a gap:** Ensure the Project feature-request item is updated (Step 2); the resolution in IMPLEMENTATION_VERIFICATION can reference that item.
- [ ] **When verified in product:** Update status to ✅ Verified in the Resolved row if your project uses that convention; otherwise Resolution is enough.

**Output:** Row moved to Resolved Questions; Project feature-request item updated if a gap was confirmed.

---

### Step 5: Update Documentation When Verified

- [ ] **If behavior was verified as implemented:** Update the relevant subsystem doc in `docs/03-architecture/` or `docs/04-behavior/` to match what you found (or note "verified YYYY-MM-DD" where useful).
- [ ] **If a gap was confirmed:** Documentation and Project feature-request item "Not Yet Implemented" already reflect it; no further doc change required for the gap itself beyond the feature-request item.

**Output:** Docs stay aligned with verified implementation; gaps stay in Project feature-request items until implemented.

---

### Step 6: When Updating Status Documents (CURRENT_STATUS, SOW Deliverables, Project Feature Requests)

When you update **status documentation** to reflect verified state (e.g. deploy verified, deliverable marked done), provide a **comprehensive understanding of what was done and why** so status is trackable and useful for future reference.

**Applies to:** `docs/08-status/CURRENT_STATUS.md`, `docs/08-status/scope-of-work/SCOPE_OF_WORK.md` (deliverables §6, work items, or "current state" notes), GitHub Project Feature Requests view (status verification).

**Required when updating these docs to reflect verified state:**

- [ ] **What was done** — State concretely what was verified or reconciled (e.g. "Deployment status reconciled with Render API; four services and Postgres confirmed live").
- [ ] **Why it matters** — State why recording this supports status tracking or future reference (e.g. "Production-ready condition #1 requires deployed and reachable services; avoids duplicate deploy work").
- [ ] **How it was verified** — State the verification method and date (e.g. "2026-01-28: Render API with RENDER_API_KEY from .env.render; GET /v1/services, /deploys, /postgres; health and static URL checks"). Link to the procedure (e.g. [Render API Access Setup](../RENDER_API_ACCESS_SETUP.md)) when applicable.
- [ ] **When** — Record the verification date (YYYY-MM-DD).
- [ ] **Where to see details** — Point to the authoritative section (e.g. "See CURRENT_STATUS §1") so readers know where to get current state.

**Authority:** `docs/08-status/CURRENT_STATUS.md` is the single source of truth for current implementation and deployment status. When marking SOW deliverables done or updating Project Feature Request items, link to CURRENT_STATUS §1 (or the relevant section) so future readers and agents use one place for current state.

**Output:** Status docs that support comprehensive understanding, status tracking, and future reference; updates are traceable (what, why, how, when).

---

## Verification Areas (Reference)

Common areas to verify, per existing IMPLEMENTATION_VERIFICATION content:

- **Authentication flows** — JWT, OAuth, session management
- **User roles and permissions** — Role-based access control
- **API endpoints** — Endpoints match documentation
- **Database schema** — Migrations match schema docs
- **UI components** — Screens match behavior docs
- **External integrations** — the state voter lookup, payment processors, notifications

Use these as prompts when adding Open Questions; do not limit questions to this list.

---

## Integration with Other Processes

- **AI.md:** "When finding implementation gaps" instructs agents to verify gaps, update Project feature-request items, and record in IMPLEMENTATION_VERIFICATION.md; "When updating status documentation" instructs agents to record what/why/how/when when updating CURRENT_STATUS, SOW, or Project items. Both point to this SOP.
- **Feature requests:** Gaps must be in the relevant Project feature-request item's "Not Yet Implemented" (or equivalent) section. No verification item is "done" if a confirmed gap is untracked there.
- **Documentation Process:** When verification confirms behavior, update subsystem docs and System Map per DOCUMENTATION_PROCESS.md.
- **Scope of Work SOP:** When marking SOW deliverables done, record why and how verified (or link to CURRENT_STATUS) per Step 5 of that SOP; status-doc updates follow this SOP Step 6.
- **Backend / Screen Examination:** Those SOPs may generate questions that are then tracked and resolved via this SOP.

---

## Notes

- **One source of truth for verification questions:** IMPLEMENTATION_VERIFICATION.md. Do not keep a separate shadow list; all open/resolved verification questions live there.
- **Gaps vs. questions:** A "question" may be "is X implemented?" — if the answer is no, that is a gap and must appear in a Project feature-request item.
- **Verification session:** Update "Verification Session" or "Last Updated" in IMPLEMENTATION_VERIFICATION.md when you add or resolve questions so readers know currency.

---

**Last Updated:** 2026-01-27
