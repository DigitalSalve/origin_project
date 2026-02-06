# Gap Identification — SOP

**Purpose:** Standard Operating Procedure for systematically identifying and classifying **feature gaps**, **infrastructure gaps**, and **scope gaps** in a project. This SOP provides a structured approach to discovering what is missing, what needs to be built, and what should be explicitly excluded from the project scope.

**Status:** Active

**Applies To:** AI agents, developers, product managers, and anyone performing gap analysis, scope definition, or project planning.

---

## Overview

This SOP ensures that:

1. **Gap types are clearly distinguished** — Feature gaps (missing functionality), infrastructure gaps (missing foundational systems), and scope gaps (boundary questions) are identified and classified separately.
2. **Systematic identification** — Gaps are discovered through structured examination (screen, backend, user flows), documentation review, and verification processes.
3. **Proper tracking** — Each gap type is tracked in the appropriate location (feature requests, infrastructure docs, scope of work).
4. **Boundary clarity** — Scope gaps are resolved by explicitly defining what is in-scope vs out-of-scope, preventing scope creep and clarifying product boundaries.

---

## Prerequisites

Before beginning gap identification:

1. **Read relevant documentation:**
   - `AI.md` (repository-specific AI behavior rules)
   - `docs/README.md` (documentation navigation)
   - `docs/02-canon/PROJECT_SCOPE.md` (product boundaries — authoritative source for scope decisions)
   - `docs/08-status/scope-of-work/SCOPE_OF_WORK.md` (current scope of work, if it exists)

2. **Understand the project context:**
   - **Project scope** — What is currently defined as in-scope vs out-of-scope (see Project Scope document)
   - **Feature requests** — What work is already planned and tracked
   - **Examination reports** — What gaps have already been identified (screen-examination/, backend-examination/)
   - **Implementation verification** — What questions are already open

3. **Identify the source of gaps:**
   - Are you examining code and finding missing implementations?
   - Are you reviewing documentation and finding inconsistencies?
   - Are you planning work and encountering unclear boundaries?
   - Are you verifying implementation and finding gaps?

---

## Gap Types

### 1. Feature Gaps

**Definition:** Missing functionality or features that are **within the project scope** but not yet implemented.

**Characteristics:**
- Documented in behavior docs (`docs/04-behavior/`) or feature requests
- Expected by users or stakeholders
- Part of the intended product experience
- Can be tracked as a feature request

**Examples:**
- Screen or flow documented but not implemented
- API endpoint documented but missing
- User-facing feature planned but not built
- Stubbed functionality (placeholder that needs real implementation)

**Tracking Location:**
- **Primary:** Project items (label `coordination:feature-request`); spec in item body; "Not Yet Implemented" in item
- **Legacy:** `docs/08-status/feature-requests/` (deprecated; use Project)

**Identification Sources:**
- Screen examination reports (stubbed screens, mock data)
- Backend examination reports (missing routes, incomplete flows)
- User flow identification (documented flows not implemented)
- Implementation verification (documented but not found in code)
- Behavior docs (flows documented but missing)

---

### 2. Infrastructure Gaps

**Definition:** Missing foundational systems, services, or technical capabilities required to support features but not themselves user-facing features.

**Characteristics:**
- Technical foundation (not user-facing)
- Required for features to work
- Cross-cutting concern (used by multiple features)
- May be reusable across projects

**Examples:**
- Missing service layer architecture
- Missing API client generation
- Missing authentication infrastructure
- Missing deployment configuration
- Missing database migrations
- Missing platform abstraction (mobile vs web)
- Missing state management setup
- Missing error handling infrastructure

**Tracking Location:**
- **Primary:** Project items (label `coordination:feature-request`) for infrastructure work
- Architecture docs (`docs/03-architecture/`)
- Infrastructure guides (`docs/07-guides/INFRASTRUCTURE_*.md`)
- Feature request TODO (marked as infrastructure/component)

**Identification Sources:**
- Architecture review (missing system components)
- Service layer examination (missing services)
- Deployment review (missing deploy config)
- Technology conversion plans (missing infrastructure for new stack)

---

### 3. Scope Gaps

**Definition:** Unclear boundaries about what is **in-scope** vs **out-of-scope** for the project. These are questions about product boundaries, not missing implementations.

**Characteristics:**
- Questions about "should this be included?"
- Boundary decisions (in-scope vs out-of-scope)
- Product definition questions
- Feature prioritization decisions
- Migration scope questions (what to migrate vs leave behind)

**Examples:**
- "Should we migrate feature X from source?"
- "Is payment processing in scope?"
- "Should admin feature Y be included in Phase 1?"
- "Do we need to support platform Z?"
- "Should we implement feature A that exists in source but isn't documented?"

**Tracking Location:**
- Scope of Work (`docs/08-status/scope-of-work/SCOPE_OF_WORK.md`)
- "In scope" vs "Out of scope" sections
- Project scope document (see below)
- Feature request decisions (explicitly scoped in or out)

**Identification Sources:**
- Scope of Work review
- Migration planning (what to migrate)
- Feature request review (unclear boundaries)
- Stakeholder questions
- Comparison with source (if migrating)

---

## Process

### Step 1: Identify Gaps Through Examination

- [ ] **Screen examination** — Run [Screen Examination SOP](../examination/SCREEN_EXAMINATION_SOP.md); identify stubbed screens, mock data, missing API integration
- [ ] **Backend examination** — Run [Backend Examination SOP](../examination/BACKEND_EXAMINATION_SOP.md); identify missing routes, incomplete flows, infrastructure needs
- [ ] **User flow identification** — Run [User Flow Identification SOP](../examination/USER_FLOW_IDENTIFICATION_SOP.md); identify documented flows not implemented
- [ ] **Implementation verification** — Run [Implementation Verification SOP](../coordination/IMPLEMENTATION_VERIFICATION_SOP.md); identify documented features not found in code
- [ ] **Architecture review** — Review `docs/03-architecture/`; identify missing system components, services, infrastructure
- [ ] **Documentation review** — Review behavior docs (`docs/04-behavior/`); identify documented behavior not implemented

**Output:** List of potential gaps (not yet classified)

---

### Step 2: Classify Gaps by Type

For each gap identified in Step 1, classify it using the decision tree below:

#### Decision Tree

**Question 1: Is this a question about product boundaries (should this be included)?**
- **Yes** → This is a **Scope Gap**. Go to Step 4 (Resolve Scope Gaps).
- **No** → Continue to Question 2.

**Question 2: Is this missing user-facing functionality that is within scope?**
- **Yes** → This is a **Feature Gap**. Track in feature request documents.
- **No** → Continue to Question 3.

**Question 3: Is this missing foundational system/service required for features to work?**
- **Yes** → This is an **Infrastructure Gap**. Track in infrastructure feature requests or architecture docs.
- **No** → Re-evaluate: Is this actually a gap, or is it out of scope? If unclear, treat as Scope Gap.

#### Classification Checklist

- [ ] **Feature Gap?** — Missing user-facing functionality within scope?
  - ✅ **Yes** → Create Project item (label `coordination:feature-request`) or add to existing item
  - ❓ **Unclear if in scope** → Treat as Scope Gap first, then reclassify after scope decision
  - **Examples:** "Login screen uses mock data", "RSVP button is stubbed", "Candidate detail page missing"

- [ ] **Infrastructure Gap?** — Missing foundational system/service?
  - ✅ **Yes** → Track as infrastructure feature request or architecture doc
  - **Examples:** "No API client generation", "Missing service layer for platform abstraction", "Deployment config incomplete"

- [ ] **Scope Gap?** — Question about boundaries (should this be included)?
  - ✅ **Yes** → Resolve in Scope of Work or Project Scope document
  - **Examples:** "Should we migrate feature X from source?", "Is payment processing in scope?", "Do we need to support platform Z?"

**What to look for:**
- **Feature gaps** often appear as: mock data, stubbed functions, "TODO" comments, placeholder UI
- **Infrastructure gaps** often appear as: missing service abstractions, hardcoded platform-specific code, missing deployment configs
- **Scope gaps** often appear as: "Should we...?" questions, unclear boundaries, feature requests without scope clarity

**Output:** Classified gaps (Feature / Infrastructure / Scope) with clear rationale for each classification

---

### Step 3: Track Gaps Appropriately

- [ ] **Feature gaps** → Create Project item (label `coordination:feature-request`) or add to existing item
  - Location: GitHub Project (Feature Requests view)
  - Spec in item body; "Not Yet Implemented" in item
  - Legacy: `docs/08-status/feature-requests/` (deprecated; use Project)

- [ ] **Infrastructure gaps** → Create Project item (label `coordination:feature-request`) or add to architecture doc
  - Location: Project item or `docs/03-architecture/`
  - May also update: `docs/07-guides/INFRASTRUCTURE_*.md` if it's reusable infrastructure

- [ ] **Scope gaps** → Resolve in Scope of Work or project scope document
  - Location: `docs/08-status/scope-of-work/SCOPE_OF_WORK.md` (in-scope/out-of-scope sections)
  - Or: Project scope document (see below)
  - Decision: Explicitly state what is included vs excluded

**Output:** All gaps tracked in appropriate locations

---

### Step 4: Resolve Scope Gaps

For scope gaps, make explicit decisions:

- [ ] **Review project scope document** — Does it clearly define boundaries?
- [ ] **Make boundary decisions** — For each scope gap, decide: in-scope or out-of-scope
- [ ] **Document decisions** — Add to Scope of Work or project scope document
- [ ] **Update feature requests** — If a feature is out-of-scope, mark it as such or remove from TODO
- [ ] **Communicate boundaries** — Ensure scope decisions are visible in planning docs

**Output:** Clear scope boundaries documented

---

## Integration with Other Processes

- **Screen/Backend Examination SOPs** — These produce gap findings that feed into this SOP
- **Implementation Verification SOP** — Identifies implementation gaps (feature or infrastructure)
- **Scope of Work SOP** — Resolves scope gaps and documents boundaries
- **Feature Request Process** — Feature gaps become feature requests
- **Documentation Process** — Infrastructure gaps may require architecture doc updates

---

## Output Locations

| Gap Type | Primary Tracking Location | Secondary Location |
|----------|---------------------------|-------------------|
| **Feature Gap** | Project items (`coordination:feature-request`) | Project Feature Requests view |
| **Infrastructure Gap** | Project items or architecture docs | `docs/07-guides/INFRASTRUCTURE_*.md` |
| **Scope Gap** | Scope of Work (`docs/08-status/scope-of-work/SCOPE_OF_WORK.md`) or project scope doc | Feature requests (if scoped out) |

---

## Notes

- **Gap vs. Bug:** A gap is something that was never implemented; a bug is something implemented incorrectly. This SOP covers gaps, not bugs.
- **Gap vs. Enhancement:** A gap is missing functionality that should exist per docs/scope; an enhancement is new functionality beyond current scope. This SOP covers gaps; enhancements are new feature requests.
- **Scope decisions are authoritative:** Once a scope gap is resolved (in-scope or out-of-scope), that decision is authoritative until explicitly changed.

---

**Last Updated:** 2026-01-28
