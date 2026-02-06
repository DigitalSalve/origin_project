Documentation Process (Working Agreement)
========================================

Goal: keep feature docs in sync with code so we don't re-read the codebase to understand behavior.

When to Update
- New subsystem or behavior change (auth/session, user roles/permissions, admin UX, content management, elections/polling).
- New migrations that affect flows.
- User-facing changes or admin UX changes.

Where to Update
- Subsystem docs in `docs/03-architecture/` (e.g., API structure, database schema, frontend architecture) or `docs/04-behavior/` (user interactions, UI flows).
- Add a one-liner + link in `docs/03-architecture/SYSTEM_MAP.md` for anything new.
- If a new domain is introduced, add a dedicated doc and link it from the system map.

What to Capture
- What it does and why.
- Where the code lives (entry points, key components, routes).
- Data shape / tables / migrations involved.
- UI impacts (screens, filters, visibility).
- Ops impacts (restarts, env vars/policies).

Working Checklist (per feature/PR)
- [ ] Note code entry points and UI changes in the subsystem doc.
- [ ] Link it from `system-map.md`.
- [ ] Mention migrations/env/policy changes required.
- [ ] Note user-facing screen impacts if any.
- [ ] Add a feature note (or update an existing one) using `docs/07-guides/templates/FEATURE_NOTE.md`.

When Adding New Project-Agnostic Documentation
- When you add a new project-agnostic SOP, process, or infrastructure doc (e.g. new `*_SOP.md` in Layer 6, new `INFRASTRUCTURE_*.md` in Layer 7):
  1. **Add to CORE_INFRASTRUCTURE_INVENTORY** — `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md` (path, purpose, reusability).
  2. **Sync to blank** — Follow `docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md`: copy file to `~/Github/blank/[same-path]`, log in `~/Github/blank/INFRASTRUCTURE_VERSION_LOG.md`.
  3. **Link in docs/README.md** — Ensure the new doc is linked in the correct layer (and in `docs/06-operations/README.md` if under Layer 6).
- Do this immediately; do not defer to the monthly audit. See **AI.md** “Infrastructure Template Maintenance.”

## Feature Request Creation Process (Required)

**Before implementing any feature or making significant changes, you MUST create a feature request.**

**Feature requests live in the GitHub Project** — not in docs. They desync across worktrees if stored in the repo.

1. **Create Project item** with label `coordination:feature-request`:
   - **Title:** Feature name
   - **Body:** Full spec using format from `docs/07-guides/templates/FEATURE_NOTE.md` or equivalent:
     - Overview, Requirements, **Subtasks** (required), Technical Details, Dependencies
     - **Business Rationale (why now)** and **Decision Date**
   - **Subtasks (required):** Break the feature into discrete, checkable subtasks using markdown checkboxes (`- [ ]` / `- [x]`). Mark each off during implementation. When all are done, run Feature Request Completion Process.
   - Set **Project** field to project slug. **Assign to user:** use `--assignee @me` when creating via `gh issue create`. Add role labels (e.g. `agent:backend`) if applicable.

2. **Multi-repository features:**
   - Create **separate Project items per repository**; link between them in the "Related" section of each item body.

3. **During implementation:**
   - **Note deviations** in the item body (add "Implementation Notes" section) as they occur.

**Critical Rules:**
- **No implementation without a feature request** — All features must have a Project item before code changes begin
- **One feature request per repository** — If changes span multiple repos, one Project item per repo
- **Document deviations** — All deviations must be noted in the item body

**Reference:** [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md), [Migration Plan](../../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md)

## Feature Request Completion Process

A feature is **complete** when all subtasks in the Project item body are marked done (`- [x]`). Only then run this process.

When a feature request is completed (all subtasks marked off), follow this process:

1. **Update the Project item body:**
   - Add `**Completed:** YYYY-MM-DD` at top
   - Add or update `## Implementation Notes` with: completion date, summary, key files, DB changes, API endpoints

2. **Close the Project item** (or move to "Done" / set status resolved)

3. **Create feature note (optional, for versioned record):**
   - Use `docs/07-guides/templates/FEATURE_NOTE.md` template
   - Save in `docs/07-guides/`; link from `docs/README.md` Layer 7
   - This is a versioned snapshot; the Project item remains the record

**Critical:** All subtasks must be marked complete before closing. Features cannot be marked complete until all steps above are done.

## Screen Examination Process

When examining screens for broken or stubbed content, follow the systematic process defined in:

- [Screen Examination SOP](SCREEN_EXAMINATION_SOP.md) — Step-by-step procedure for identifying broken functionality, stubbed content, and incomplete implementations

**Output:** Reports go to `docs/08-status/screen-examination/`; link new reports from `docs/README.md` Layer 8.

This SOP provides:
- Systematic screen inventory process
- 8-point examination checklist per screen
- User flow testing procedures
- Issue classification by severity
- Structured reporting format
- Integration with feature request and documentation processes

## Backend Examination Process

When examining the backend for migration or verification, follow:

- [Backend Examination SOP](BACKEND_EXAMINATION_SOP.md) — Systematic process to inventory components and processes, classify working / not working / needing implementation, and produce a structured report.

**Output:** Reports go to `docs/08-status/backend-examination/`; link new reports from `docs/README.md` Layer 8.

## Related Operations (Layer 6)

To keep documentation and repo integrity:

- **Discovery flow:** `AI.md` → `docs/README.md` → layer. All operations and reports are linked from `docs/README.md`; do not create orphaned docs.
- **Daily integrity check:** Run [Repo Integrity Check SOP](REPO_INTEGRITY_CHECK_SOP.md) at least once per calendar day. If [INTEGRITY_CHECK_LOG](../08-status/INTEGRITY_CHECK_LOG.md) has **no** entry for **today** (YYYY-MM-DD), run the SOP **before** the next operation. See `AI.md` “Before making changes.”
- [Migration Process](MIGRATION_PROCESS.md) — How migration (Discover → Document → Plan → Implement → Verify) is run; examination reports feed into Document/Plan.
- [File Naming Conventions](FILE_NAMING_CONVENTIONS.md) — Use when creating new docs, reports, or folders so naming stays consistent.
- **Reports (Layer 8):** Screen examination → `docs/08-status/screen-examination/`; backend examination → `docs/08-status/backend-examination/`. Integrity check log → `docs/08-status/INTEGRITY_CHECK_LOG.md`. Other report types (see `docs/README.md` Layer 8) have their own locations; add new report types to Layer 8 and link from `docs/README.md`.
