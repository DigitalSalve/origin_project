# Scope of Work — SOP

**Purpose:** Standard Operating Procedure for creating a **Scope of Work (SOW)** from expected project requirements and for defining and executing the work to complete those requirements. This SOP is **project-agnostic**: it applies to any project using this documentation structure. It is the culmination of planning—synthesizing canon, architecture, behavior, integrations, feature requests, examination reports, intended user flows, and verification into a single SOW and a clear execution path.

**Status:** Active

**Applies To:** Any project using this doc structure when you need a formal Scope of Work that ties expected requirements to deliverables and work items, and a repeatable way to produce and maintain that SOW and execute against it.

---

## Overview

This SOP ensures that:

1. **Expected requirements** are gathered from the right sources (canon, architecture, behavior, integrations, feature requests, examination reports, intended user flows, implementation verification).
2. **A Scope of Work document** is produced that states scope, deliverables, work items, completion criteria, and ordering.
3. **Work items** are tied to feature requests and the Documentation Process so completion is tracked and documented.
4. **Execution** follows existing discipline (feature requests, DOCUMENTATION_PROCESS, coordination) so the SOW is not a separate parallel process but the **planning capstone** that drives work already governed by this structure.

The SOW is project-agnostic in structure; each project fills it with its own requirements, deliverables, and work items.

---

## When to Use This SOP

- **Planning a phase, release, or body of work** — You need a single document that states “what is in scope,” “what must be delivered,” and “what work we will do” and in what order.
- **After discovery or examination** — Screen examination, backend examination, user flow identification, or other discovery has produced reports; you want to turn those into a SOW and prioritized work items.
- **When requirements are spread across many docs** — Canon, architecture, behavior, integrations, and status (feature requests, verification) hold the “expected requirements”; you want one SOW that references them and lists concrete deliverables and work items.
- **Before or during execution** — You want to track “work to complete requirements” against the SOW and keep the SOW updated as scope or completion changes.

---

## Inputs (Expected Requirements Sources)

Gather from these sources when building the SOW. The exact set depends on the project; use what exists and is relevant.

| Source | Location | What to use |
|--------|----------|-------------|
| **Canon** | `docs/02-canon/` | System truth, invariants, compliance, audit scope—requirements that must hold |
| **Architecture** | `docs/03-architecture/` | System map, data, boundaries, state—structural and technical requirements |
| **Behavior** | `docs/04-behavior/` | Auth, API matrix, UI integration points, design system, intended user flows—behavioral requirements |
| **Integrations** | `docs/05-integrations/` | External APIs, webhooks, provider contracts—integration requirements |
| **Feature requests** | GitHub Project (label `coordination:feature-request`) | Planned features, migration plans—work already scoped as requests |
| **Examination reports** | `docs/08-status/screen-examination/`, `backend-examination/`, `user-flows/` | Findings, gaps, “working / not working / needed”—requirements and gaps inferred from discovery |
| **Implementation verification** | `docs/08-status/IMPLEMENTATION_VERIFICATION.md` | Open/resolved questions, gaps—requirements or fixes still to be done |
| **Executive/overview** | `docs/08-status/` (e.g. MIGRATION_EXECUTIVE_OVERVIEW, CURRENT_STATUS) | High-level goals, phases, and status—when the project has a named plan or migration |

Use project identity (`docs/01-orientation/PROJECT_IDENTITY.md`) for project name and context when referring to “this project” in the SOW; do not hardcode a specific product name in this SOP.

---

## Output Location and Naming

| Deliverable | Location | Naming |
|-------------|----------|--------|
| **Scope of Work (living doc)** | `docs/08-status/scope-of-work/SCOPE_OF_WORK.md` | `SCOPE_OF_WORK.md` |
| **Optional dated snapshot** | `docs/08-status/scope-of-work/` | `YYYY-MM-DD_SCOPE_OF_WORK_SNAPSHOT.md` when you want a versioned snapshot (e.g. at phase gate or release) |

**Authority:** The living SOW is `docs/08-status/scope-of-work/SCOPE_OF_WORK.md`. Dated snapshots are append-only history; they do not replace the living doc. Per [Report Creation and Delivery SOP](../conventions/REPORT_CREATION_AND_DELIVERY_SOP.md), this SOP owns the `scope-of-work/` folder and its naming.

---

## Process

### Step 1: Gather Expected Requirements

- [ ] **Canon** — Review `docs/02-canon/` and list requirements that must hold (invariants, compliance, audit).
- [ ] **Architecture** — Review `docs/03-architecture/` (e.g. SYSTEM_MAP, key docs) and list structural/technical requirements (APIs, data, boundaries).
- [ ] **Behavior** — Review `docs/04-behavior/` (auth, API matrix, UI integration points, intended user flows when present) and list behavioral requirements (flows, screens, roles).
- [ ] **Integrations** — Review `docs/05-integrations/` and list integration requirements (external APIs, webhooks).
- [ ] **Feature requests** — Review GitHub Project Feature Requests view (label `coordination:feature-request`); list planned work already scoped.
- [ ] **Examination reports** — Review latest reports in `screen-examination/`, `backend-examination/`, `user-flows/` (as applicable); extract “working,” “not working,” “needed,” and gaps as requirements or work items.
- [ ] **Implementation verification** — Review `IMPLEMENTATION_VERIFICATION.md`; list open questions and gaps that imply work.
- [ ] **Executive/overview** — If the project has a MIGRATION_EXECUTIVE_OVERVIEW, CURRENT_STATUS, or similar, use it to align scope and phases.

**Output:** A consolidated view of “expected requirements” and “already-scoped work” (feature requests). No project name or domain is assumed; use “this project” or the project name from PROJECT_IDENTITY when writing the SOW.

---

### Step 2: Define Scope and Boundaries

- [ ] **In scope** — List what is included in this SOW (e.g. by phase, release, domain, or “all requirements from Step 1”). Be explicit enough that “out of scope” is clear.
- [ ] **Out of scope** — List what is explicitly excluded (e.g. future phases, other repos, deferred items).
- [ ] **Assumptions** — List assumptions (e.g. “canon and behavior docs are current,” “feature requests exist for all in-scope work,” “examination reports reflect state as of YYYY-MM-DD”).
- [ ] **Dependencies** — List external or upstream dependencies (e.g. another repo, third-party delivery, sign-off).

**Output:** A scope section suitable for the SOW document. Project-agnostic; each project fills it with its own boundaries.

---

### Step 3: List Deliverables and Work Items

- [ ] **Deliverables** — What must exist when the SOW is “done” (e.g. “all in-scope features implemented and documented,” “all gaps from IMPLEMENTATION_VERIFICATION resolved,” “feature requests moved to completed,” “docs updated per DOCUMENTATION_PROCESS”).
- [ ] **Work items** — Concrete work items. Each item should:
  - Be tied to a **feature request** when one exists (link to Project item or doc).
  - Otherwise be stated clearly enough to create a feature request per [Documentation Process](DOCUMENTATION_PROCESS.md) before implementation.
  - Have a short **completion criterion** (e.g. “Screen X matches behavior doc,” “Endpoint Y implemented and in API matrix”).
- [ ] **Ordering** — Indicate order or dependencies (e.g. “Phase 1: A, B, C; Phase 2: D, E” or “B depends on A”). Use Project item priority when it already encodes order.

**Output:** A deliverables section and a work-item list (table or list) with links to feature requests where applicable. Project-agnostic structure; project-specific content.

---

### Step 4: Produce the SOW Document

- [ ] **Create or update** `docs/08-status/scope-of-work/SCOPE_OF_WORK.md`.
- [ ] **Required sections:**
  - **Header** — Project (use name from PROJECT_IDENTITY or “this project”), SOW date, owner or author, process: “Per docs/06-operations/processes/SCOPE_OF_WORK_SOP.md.”
  - **1. Scope and boundaries** — In scope, out of scope, assumptions, dependencies (from Step 2).
  - **2. Expected requirements (sources)** — Short pointers to canon, architecture, behavior, integrations, feature requests, examination reports, implementation verification, and any executive overview. No need to duplicate their content; reference the docs and summarize what they contribute to “expected requirements.”
  - **3. Deliverables** — What must exist when the SOW is complete (from Step 3).
  - **4. Work items** — Table or list: Work item, Feature request (link or “to be created”), Completion criterion, Order/phase.
  - **5. Execution** — “Work is executed per Documentation Process and feature-request discipline. No implementation without a feature request. Completion: update docs, feature notes, and feature-request status per DOCUMENTATION_PROCESS; close completed items in Project.”
  - **6. Revision history** — Append-only log: Date, Change (e.g. “Scope added Phase 2,” “Work item X completed”), Author or source.
- [ ] **Use clear headings** so the SOW can be scanned and linked to.

**Output:** A complete `SCOPE_OF_WORK.md` that satisfies this structure. Project-agnostic template; project-specific content.

---

### Step 5: Execute Work and Update the SOW

- [ ] **Execute work** per [Documentation Process](DOCUMENTATION_PROCESS.md): create Project items (label `coordination:feature-request`) before implementation, update docs and feature notes on completion, close completed items in Project.
- [ ] **Coordinate** per [Active Work SOP](../coordination/ACTIVE_WORK_SOP.md) when editing files.
- [ ] **Update the SOW** when scope or completion changes: add a revision-history entry; optionally add “Completed” or “In progress” to work items in section 4; when a phase or the whole SOW is done, add a short “Status: Complete for Phase N” or “Status: Complete” and the date.
- [ ] **When marking a deliverable done (e.g. in §6 Deliverables):** Record **why** it was marked done and **how** it was verified (or link to `docs/08-status/CURRENT_STATUS.md` §1 or the relevant section) so status is traceable and useful for future reference. Follow [Implementation Verification SOP](../coordination/IMPLEMENTATION_VERIFICATION_SOP.md) Step 6 (status document updates).
- [ ] **Optional snapshot** — When closing a phase or releasing, copy the current SOW to `YYYY-MM-DD_SCOPE_OF_WORK_SNAPSHOT.md` in the same folder so you have a dated snapshot; continue maintaining the living `SCOPE_OF_WORK.md`.

**Output:** Work proceeds in line with existing process; the SOW stays current and traceable; deliverable completion is documented with what/why/how/when for future reference.

---

## SOW Document Template (Minimal)

Use this structure when creating or updating `docs/08-status/scope-of-work/SCOPE_OF_WORK.md`:

```markdown
# Scope of Work

**Project:** [Use PROJECT_IDENTITY or "this project"]
**SOW date:** YYYY-MM-DD
**Process:** Per docs/06-operations/processes/SCOPE_OF_WORK_SOP.md

---

## 1. Scope and boundaries

- **In scope:** …
- **Out of scope:** …
- **Assumptions:** …
- **Dependencies:** …

## 2. Expected requirements (sources)

- **Canon:** docs/02-canon/ — [one-line summary]
- **Architecture:** docs/03-architecture/ — [one-line summary]
- **Behavior:** docs/04-behavior/ — [one-line summary]
- **Integrations:** docs/05-integrations/ — [one-line summary or "N/A"]
- **Feature requests:** Project (label `coordination:feature-request`) — [one-line summary]
- **Examination reports:** screen-examination/, backend-examination/, user-flows/ — [one-line summary]
- **Implementation verification:** IMPLEMENTATION_VERIFICATION.md — [one-line summary]
- **Executive/overview:** [e.g. MIGRATION_EXECUTIVE_OVERVIEW or CURRENT_STATUS] — [one-line summary if applicable]

## 3. Deliverables

- [ ] Deliverable 1
- [ ] Deliverable 2
- …

## 4. Work items

| Work item | Feature request | Completion criterion | Order/phase |
|-----------|-----------------|---------------------|-------------|
| … | link or "to create" | … | … |

## 5. Execution

Work is executed per Documentation Process and feature-request discipline. No implementation without a feature request. On completion: update subsystem docs, system map, feature notes; close completed items in Project.

## 6. Revision history

| Date | Change | Author/source |
|------|--------|---------------|
| YYYY-MM-DD | Initial SOW | … |
```

---

## Integration with Other Processes

- **Documentation Process** — All work items that entail implementation must have a feature request; completion follows DOCUMENTATION_PROCESS (docs, feature notes, move to completed, update TODO).
- **Feature requests** — The SOW references and orders work already captured (or to be captured) as feature requests; it does not replace the feature-request lifecycle.
- **Examination SOPs** — Screen, backend, and user-flow examinations produce reports that feed “expected requirements” and gaps into the SOW; their output locations remain as defined in those SOPs.
- **Implementation Verification SOP** — Open questions and gaps in IMPLEMENTATION_VERIFICATION feed into the SOW as requirements or work items; verification is updated as work completes. When marking deliverables done, record why and how verified (or link to CURRENT_STATUS) per that SOP Step 6 (status document updates).
- **Migration Process** — When the project is a migration, the SOW can align with Discover → Document → Plan → Implement → Verify; the SOW is the “Plan” capstone that ties requirements to work items and execution.
- **Active Work SOP** — Coordination (check-out/check-in) applies when editing any file, including SOW and feature requests.
- **Report Creation and Delivery SOP** — This SOP owns `docs/08-status/scope-of-work/`; naming and placement follow the pattern above. Optional dated snapshots use `YYYY-MM-DD_SCOPE_OF_WORK_SNAPSHOT.md`.

---

## Notes

- **Project-agnostic** — This SOP does not assume migration, a specific product, or a specific domain. “Expected requirements” and “work items” are defined by the project’s canon, architecture, behavior, integrations, and status. The SOW structure is fixed; the content is project-specific.
- **Culmination of planning** — The SOW synthesizes what already exists in layers 2–5 and 08-status into one place so “scope,” “deliverables,” and “work to do” are explicit and traceable.
- **Single living doc** — Prefer one `SCOPE_OF_WORK.md` updated over time; use dated snapshots only when you need a versioned record (e.g. at phase gate or release).

---

**Last updated:** 2026-01-27
