# Migration Process

**Purpose:** Define how migration from the original source application to this repository is performed. This document is the **process** (the way we work); specific migration plans (what to migrate, technology choices, timelines) live in feature requests and status.

**Last Updated:** (set when approved)

---

## Scope: Source vs Adaptation

### Original source is left alone

- **Original source:** The application in the **gop_app** directory (or the repository that contains it) is **read-only** for migration purposes.
- **No adaptation work is performed in the source.** We do not modify, refactor, or "adapt in place" the original codebase.
- **Use of source:** Reference only. We read it to discover behavior, UI, APIs, and data; we do not change it as part of migration.

### All adaptation work is in this repository (this project)

- **Adaptation work** is performed **only in the this project repo directory**.
- Documentation, migration plans, specs extracted from the source, and any new or adapted code (when we add code to this project) live here.
- **Tracking:** Feature requests, status, and migration progress are maintained in `docs/08-status/` and related docs in this repo.

**Summary:** Source = read-only reference. This project's repo = where we document the process, plan migrations, and perform all adaptation work.

---

## Process Overview

Migration is performed in phases. Each phase produces artifacts **in this repository**; the source is consulted only as reference.

| Phase | Purpose | Where work happens | Source (gop_app) |
|-------|---------|--------------------|-------------------|
| **1. Discover** | Understand behavior, screens, APIs, data in the source | this project docs (e.g. screen examination, notes) | Read-only reference |
| **2. Document** | Capture specs, flows, and requirements in this project | this project docs/02-canon, 03-architecture, 04-behavior, 08-status | Read-only reference |
| **3. Plan** | Decide what to migrate, in what order, and how | this project feature requests, migration plans | Not modified |
| **4. Implement** | Build or adapt in this project (docs first; code when applicable) | this project | Not modified |
| **5. Verify** | Confirm behavior matches intent derived from source | this project QA docs, verification logs | Read-only comparison |

---

## Phase 1: Discover

**Goal:** Learn what the source does without changing it.

**Steps:**

1. **Use the source as reference only.** Run, inspect, and read gop_app code and UI. Do not commit changes to the source as part of migration.
2. **Record findings in this project.** Use:
   - `docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md` for systematic screen-by-screen examination.
   - `docs/08-status/screen-examination/` for examination reports.
   - `docs/08-status/` for gaps, mock data, stubbed behavior, and API usage.
3. **Capture behavior and structure.** Document screens, flows, APIs, and data usage in this project docs (orientation, canon, architecture, behavior) so the process does not depend on memory or re-reading the source for basics.

**Outputs (all in this project):** Examination reports, gap lists, and draft docs that describe what the source does.

---

## Phase 2: Document

**Goal:** Turn discoveries into stable specs and requirements in this project.

**Steps:**

1. **Write or update docs in this project** in the appropriate layers:
   - **Canon / system truth** — What the system is, invariants, compliance (in `docs/02-canon/` or project equivalent).
   - **Architecture** — Structure, data, boundaries (in `docs/03-architecture/` or equivalent).
   - **Behavior** — User flows, API usage, UI integration (in `docs/04-behavior/` or equivalent).
2. **Keep source as reference.** When documenting, cite "per gop_app [path/screen]" where useful, but the **authoritative description** lives in this project.
3. **List migration requirements.** In `docs/08-status/` (e.g. feature requests, CURRENT_STATUS, IMPLEMENTATION_VERIFICATION), record what must hold true for a migration to be correct (screens, APIs, data, roles, etc.).

**Outputs (all in this project):** Canon, architecture, and behavior docs; migration requirements and acceptance criteria.

---

## Phase 3: Plan

**Goal:** Decide what to migrate, in what order, and how.

**Steps:**

1. **Create or update migration plan(s)** as Project items (label `coordination:feature-request`). Use one (or more) Project items to describe:
   - Scope of migration (e.g. by screen, flow, or module).
   - Order of work (dependencies, risk, value).
   - Technology and design choices for the **adapted** system (e.g. stack, state management, API shape).
   - Dependencies (e.g. backend endpoints, design system).
2. **Track in Project.** Add each migration plan or sub-plan as a Project item; use priority/status fields.
3. **Do not change the source.** Planning and prioritization are about work done **in this project**; the source remains read-only.

**Outputs (all in this project):** Migration feature request(s), todo list entries, and clear scope/order/decisions.

---

## Phase 4: Implement

**Goal:** Do the adaptation work in this project.

**Steps:**

1. **All implementation happens in this project.** New or adapted code, config, and assets live in this repository (or in a repo that this project explicitly owns for the migrated product). No “adapt in place” in the source.
2. **Follow DOCUMENTATION_PROCESS.** For each implemented unit:
   - Update subsystem docs (e.g. 03-architecture, 04-behavior).
   - Update system map when it exists.
   - Document migrations, env, and UI impacts.
   - Create a feature note per `docs/07-guides/templates/FEATURE_NOTE.md`.
3. **Check out before editing.** Use `ACTIVE_WORK.md` and `ACTIVE_WORK_LOG.md` as in `AI.md`.
4. **Reference source only.** When re-checking behavior, read or run the source; do not edit it as part of this phase.

**Outputs (all in this project):** Implemented features, updated docs, feature notes, and completed/moved feature requests per DOCUMENTATION_PROCESS.

---

## Phase 5: Verify

**Goal:** Confirm that adapted behavior matches the intended behavior derived from the source.

**Steps:**

1. **Compare against documented intent.** Use this project docs (canon, behavior, feature requests) as the definition of “correct.” Use the source only as a read-only reference for how the original behaved.
2. **Record verification in this project.** Use `docs/08-status/IMPLEMENTATION_VERIFICATION.md` and QA docs (e.g. `docs/08-status/qa/`) to log what was checked and any gaps.
3. **Treat gaps as this project work.** If something is missing or wrong, the fix is planned and implemented in this project, not by changing the source.

**Outputs (all in this project):** Verification notes, QA logs, and any new feature requests or bug-fix items for adaptation work.

---

## Checklist: One Migration Unit

Use this for a single screen, flow, or module:

- [ ] **Discover:** Examined source (read-only); findings recorded in this project (e.g. screen examination or notes).
- [ ] **Document:** Spec/requirements for this unit written in this project (canon/architecture/behavior as needed).
- [ ] **Plan:** Unit is covered by a Project item (label `coordination:feature-request`) for the migration plan.
- [ ] **Implement:** Work done in this project; docs and feature notes updated per DOCUMENTATION_PROCESS; source unchanged.
- [ ] **Verify:** Behavior checked against this project spec; results recorded in IMPLEMENTATION_VERIFICATION or QA docs.

---

## Relation to Other Processes

- **MIGRATION_LOCAL_PHASE.md** — [Migration: Work in Isolation of Render](MIGRATION_LOCAL_PHASE.md) lists migration work that can be done without a Render account or deploy (local backend, DB, render.yaml in repo, env template, etc.). Use it to progress on Implement phase before Render is connected.
- **DOCUMENTATION_PROCESS.md** — When we implement in this project, we follow the doc-update and feature-request completion steps there. Migration does not change that.
- **SCREEN_EXAMINATION_SOP.md** — Use this during **Discover** to systematically go through the source UI. Reports live under `docs/08-status/screen-examination/` in this project.
- **Feature requests** — Migration plans and sub-plans are Project items (label `coordination:feature-request`). They are created, updated, and closed per the usual feature-request lifecycle.
- **AI.md** — Coordination (ACTIVE_WORK), feature-request discipline, and documentation enforcement apply to all work in this project, including migration.

---

## When to Use This Process

- Use this process whenever work is framed as **migration from the original source (gop_app)** into this repository.
- If the scope later expands to other sources or repos, this process can be extended to name them and keep the same rule: **source read-only; all adaptation in this project (or a repo this project explicitly owns).**

---

## Maintaining This Document

This is the **process** for migration. Updates should:

- Keep the **source read-only / adaptation in this project** rule explicit.
- Preserve the five phases (Discover, Document, Plan, Implement, Verify) unless the team agrees to a different model.
- Add or adjust checklists and pointers to other docs (e.g. new SOPs) without changing the scope rule.

Changes to this process follow the same documentation discipline: update this file, then add a brief note in CURRENT_STATUS or a feature note if it was driven by a migration task.
