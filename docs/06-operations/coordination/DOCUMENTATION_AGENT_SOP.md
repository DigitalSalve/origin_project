# Documentation Agent — SOP

**Purpose:** Standard Operating Procedure for the **Documentation Agent** role: ensure documentation is up to date **before** file changes are committed to the repo. This SOP defines when to run, what to check, and what to update so the chain **code → docs → spine** stays intact.

**Status:** Active

**Applies To:** Any agent or human acting as “Documentation Agent” before committing code or doc changes, and any committer who wants to verify docs are in sync before push.

**Authority:** Full documentation lifecycle (when to create feature requests, when to update docs, completion checklist) is in [Documentation Process](../processes/DOCUMENTATION_PROCESS.md) and **AI.md** (“Documentation Process Enforcement”, “Feature Request Completion Process”). This SOP is the **pre-commit** runbook for the Documentation Agent role.

---

## Role

The **Documentation Agent** is responsible for:

1. **Complete thoughts only:** Create or update docs only for **complete thoughts or solutions** — not partial work, in-progress drafts, or unresolved questions. Documentation should capture finished, coherent outcomes.
2. **Project Q&A → docs:** Take responses from Project items (label `coordination:q-a`) and create/update docs. Implementation agents (Backend, Web, Webapp, Mobile) answer in Project; Documentation Agent turns those answers into canon, architecture, behavior, or feature notes. Do not answer implementation questions yourself; those go to the domain agent. **Modes:** Documentation Agent can run **directly** (standalone) or as **sub-agent** of an implementation agent—when run as sub-agent, it keeps the implementation context when creating docs.
3. **Before commit:** Running the Pre-Commit Documentation Checklist (below) whenever file changes (code or docs) are about to be committed.
4. **Correlation:** Ensuring that code/system changes are reflected in the right docs (Layer 3–4, System Map, feature notes) and that no doc is orphaned (everything discoverable via `docs/README.md`).
5. **Not duplicating:** Not redefining the documentation process—only executing the checklist that gates “ready to commit” from a documentation standpoint.

**Infrastructure lock:** Only the Architect Agent may create, modify, or delete infrastructure files (see [CORE_INFRASTRUCTURE_INVENTORY](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md)). The Documentation Agent must **not** change infrastructure files. If infrastructure needs to be added or updated, create a Project item (label `coordination:q-a`, target `agent:architect`) or invoke the Architect as sub-agent.

---

## When to Run

Run the Pre-Commit Documentation Checklist when **any** of the following are true and you are about to commit:

- **Project Q&A responses:** Resolved Project items (label `coordination:q-a`) with implementation-agent answers — create/update docs from those responses.
- Code changes (app, API, migrations, config)
- New or removed docs or doc folders
- Changes that add or change user-facing behavior, APIs, or data shape
- Completion of a feature (see also [Documentation Process — Feature Request Completion](../processes/DOCUMENTATION_PROCESS.md))

You may **skip** the checklist only for commits that are purely typo/formatting fixes in docs with no behavioral or structural effect.

---

## Pre-Commit Documentation Checklist

Use this checklist **before** committing. If the change set includes code or structural doc changes, every checked item that **applies** must be done; N/A is allowed when the change does not touch that area.

### 1. Feature request (if this is a feature or significant change)

- [ ] A feature request exists in the GitHub Project (label `coordination:feature-request`) for this work (**AI.md**: no implementation without a feature request).
- [ ] Deviations from the plan are recorded in that feature request’s “Implementation Notes”.

### 2. Subsystem and behavior docs ( Layers 3–4 )

- [ ] **Subsystem doc:** Code entry points and UI changes are noted in the relevant doc in `docs/03-architecture/` or `docs/04-behavior/` ([Documentation Process](../processes/DOCUMENTATION_PROCESS.md) — “Where to Update”).
- [ ] **System Map:** A one-liner and link exist in `docs/03-architecture/SYSTEM_MAP.md` for anything new ([Documentation Process](../processes/DOCUMENTATION_PROCESS.md), **AI.md** “Documentation Process Enforcement”).

### 3. Data, env, and ops

- [ ] **Migrations / env / policy:** Any new or changed migrations, env vars, or policy are mentioned in the relevant subsystem doc or feature note.
- [ ] **UI impacts:** User-facing screen impacts, filters, or controls are documented where appropriate (subsystem doc or feature note).

### 4. Feature note (when the change is a discrete feature or flow change)

- [ ] A feature note has been added or updated using `docs/07-guides/templates/FEATURE_NOTE.md`, placed in `docs/07-guides/`, and linked from `docs/README.md` Layer 7 if it’s a new file (**AI.md** “Documentation Process Enforcement”).

### 5. Completion only — feature request lifecycle

If this commit **completes** a feature:

- [ ] **Completed** date and implementation notes are in the Project feature-request item body.
- [ ] Project item has been closed (or moved to Done / status resolved).

### 6. Discoverability

- [ ] **No orphaned docs:** Any new doc or folder is linked from `docs/README.md` in the correct layer (**AI.md** “When creating new documentation structures”).
- [ ] **06-operations index:** If a new SOP or process was added under `docs/06-operations/`, it is listed in `docs/06-operations/README.md` and in `docs/README.md` Layer 6.

### 7. New project-agnostic infrastructure (when applicable)

If this commit **adds new project-agnostic documentation** (e.g. new SOP in Layer 6, new `INFRASTRUCTURE_*.md` in Layer 7):

- [ ] **Architect to add** — Do **not** add to CORE_INFRASTRUCTURE_INVENTORY or sync to origin_project yourself. Only the Architect Agent may change infrastructure files. Create a Project item (label `coordination:q-a`, target `agent:architect`) requesting: add to inventory, sync to origin_project. Or invoke Architect as sub-agent.
- [ ] **Note in commit** — If you cannot reach Architect before commit, add to commit message: `[Infrastructure: Architect to add to CORE_INFRASTRUCTURE_INVENTORY and sync to origin_project]`.

**Rule:** New project-agnostic infrastructure must be added to the inventory and synced to origin_project by the Architect; Documentation Agent defers.

---

## Failure and Escalation

- **Checklist item not done (and applicable):** Do not commit until that item is completed or explicitly deferred with a documented reason (e.g. “Document in follow-up PR” recorded in the Project feature-request item or a brief note in the commit message).
- **Unclear where a change belongs:** Prefer updating an existing doc over creating a new one. If placement is ambiguous, see **AI.md** “When to stop and ask” — stop and ask rather than guessing.
- **Contradiction with canon or architecture:** Do not commit until the contradiction is resolved or canon is updated by the right authority.

---

## References

- **AI.md** — “Documentation Process Enforcement”, “Feature Request Completion Process”, “When creating new documentation structures”, “Before making changes”
- [Documentation Process](../processes/DOCUMENTATION_PROCESS.md) — When/where to update, working checklist, feature request creation and completion
- [Infrastructure Template Sync SOP](../processes/INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md) — When adding new project-agnostic infrastructure, sync to origin_project and update inventory
- [docs/README.md](../../README.md) — Navigation spine; all new docs must be linked in the appropriate layer
- [Repo Integrity Check SOP](REPO_INTEGRITY_CHECK_SOP.md) — Run at least once per calendar day; if there is no log entry for today, run **before** the next operation (including pre-commit doc checks)
