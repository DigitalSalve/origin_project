# Active Work — SOP

**Purpose:** Standard Operating Procedure for checking out and checking in files/areas before and after work, to prevent conflicts and coordinate agents. Defines format, timing, conflict handling, and archive policy for `ACTIVE_WORK.md` and `ACTIVE_WORK_LOG.md`.

**Status:** Active

**Applies To:** AI agents and humans performing any work that touches files or areas tracked for coordination. The “check ACTIVE_WORK before editing, check-out when starting, check-in when done” rule is enforced via `docs/README.md` (Process and behavior reference); AI.md points there.

**Modes:** **GitHub mode** (when Project configured): use Project Active Work view. **Docs mode:** use `ACTIVE_WORK.md` and `ACTIVE_WORK_LOG.md`. See [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md). Check-out is optional when using worktrees.

---

## GitHub mode (when Project is configured)

1. **Before editing (optional):** Check GitHub Project → Active Work view.
2. **Check-out:** Create Project item in Active Work view. Title: `File/Area | Agent | Description`. Label: `coordination:active-work`. Set **Project** field to project slug. Assign to user.
3. **Check-in:** Close Project item or move to Done.

---

## Docs mode (ACTIVE_WORK.md / ACTIVE_WORK_LOG.md)

**Overview:** Use when GitHub Project is not yet configured.

This SOP ensures that:

1. **Coordination** — Before editing, agents check `docs/08-status/ACTIVE_WORK.md` to see if files/areas are already checked out.
2. **Check-out on start** — When starting work, agents add an entry to “Currently Active Work” in `ACTIVE_WORK.md` and append a CHECK-OUT entry to `ACTIVE_WORK_LOG.md`.
3. **Check-in on completion** — When work is complete, agents remove the entry from `ACTIVE_WORK.md` and append a CHECK-IN entry to `ACTIVE_WORK_LOG.md`.
4. **Consistent format** — Timestamps use `YYYY-MM-DD HH:MM:SS`; agent identifier, file/area path, description, and expected completion time are recorded per this SOP.
5. **Conflict handling** — If a needed file/area is already checked out, follow the conflict-resolution steps below; do not check out over another agent without resolving.
6. **Append-only log** — `ACTIVE_WORK_LOG.md` is append-only; entries are never removed. Archive policy applies when the log exceeds the threshold.

---

## File Structure and Locations

| File | Purpose | Mutability |
|------|---------|------------|
| `docs/08-status/ACTIVE_WORK.md` | Currently checked-out files/areas only; coordination view | Entries added on check-out, removed on check-in |
| `docs/08-status/ACTIVE_WORK_LOG.md` | Full history of all check-outs and check-ins | Append-only; never remove entries |

---

## Prerequisites

Before performing check-out or check-in:

1. **Read:** `docs/README.md` (Process and behavior reference) and this SOP.
2. **Open** `docs/08-status/ACTIVE_WORK.md` to see current check-outs.
3. **Open** `docs/08-status/ACTIVE_WORK_LOG.md` when appending (check-out or check-in).

---

## Process

### Check-Out (Before Starting Work)

- [ ] **Check** `ACTIVE_WORK.md` — Is the file/area you need already listed under “Currently Active Work”?
- [ ] **If yes:** See [Conflict Handling](#conflict-handling) below. Do not add a duplicate check-out for the same file/area.
  - [ ] **Identify feature request** (if work is for a feature): See GitHub Project → Feature Requests view (label `coordination:feature-request`); link to Project item in description.
- [ ] **If no:** Proceed:
  - [ ] **Add** one row to the “Currently Active Work” table in `ACTIVE_WORK.md` with:
    - **File/Area** — Full path or clear area (e.g. `docs/06-operations/`, `app/(tabs)/`)
    - **Agent** — Identifier (e.g. “Documentation Agent”, “Backend Agent”, “Migration Agent”)
    - **Checked Out** — Timestamp in `YYYY-MM-DD HH:MM:SS`
    - **Description** — Brief description of the work
    - **Feature request** — When work is for a feature: link to Project item (label `coordination:feature-request`). Blank if not applicable.
    - **Expected Completion** — Time or duration (e.g. “2h”, “EOD”)
  - [ ] **Append** one row to `ACTIVE_WORK_LOG.md` with:
    - **Timestamp** — `YYYY-MM-DD HH:MM:SS` (same as check-out)
    - **Action** — `CHECK-OUT`
    - **File/Area** — Same as in ACTIVE_WORK
    - **Agent** — Same as in ACTIVE_WORK
    - **Description** — Same as in ACTIVE_WORK
    - **Duration** — leave `-` or blank for check-out

**Output:** One new row in ACTIVE_WORK “Currently Active Work,” one new append in ACTIVE_WORK_LOG.

---

### Check-In (When Work Is Complete)

- [ ] **Remove** the corresponding row from “Currently Active Work” in `ACTIVE_WORK.md` (same File/Area and Agent as at check-out).
- [ ] **Append** one row to `ACTIVE_WORK_LOG.md` with:
  - **Timestamp** — `YYYY-MM-DD HH:MM:SS` (check-in time)
  - **Action** — `CHECK-IN`
  - **File/Area** — Same as at check-out
  - **Agent** — Same as at check-out
  - **Description** — Same as at check-out (or “Completed: …” if useful)
  - **Duration** — Elapsed time (e.g. “15m”, “2h 30m”)

**Output:** Entry removed from ACTIVE_WORK; one new append in ACTIVE_WORK_LOG with CHECK-IN and duration.

---

### Conflict Handling

If the file/area you need is already listed in “Currently Active Work”:

1. **Check** expected completion time and agent.
2. **Options:**
   - **Wait** — If completion is soon and acceptable, wait and re-check ACTIVE_WORK before proceeding.
   - **Contact / coordinate** — If the agent is known, coordinate before touching the same file/area.
   - **Document** — If you must proceed (e.g. explicit handoff or override), append a log entry with action `CONFLICT-RESOLUTION` or a short note in the CHECK-IN of the prior agent, then proceed only after explicit resolution. Do not silently check out over another agent.
3. **Log resolution** — When a conflict is resolved, append to `ACTIVE_WORK_LOG.md` with timestamp, action `CONFLICT-RESOLUTION`, file/area, agents involved, and one-line outcome.

**Rule:** Do not add a second check-out for the same file/area while another agent’s entry remains in ACTIVE_WORK unless conflict resolution is documented.

---

## Format Requirements

| Field | Format | Example |
|-------|--------|---------|
| Timestamp | `YYYY-MM-DD HH:MM:SS` | `2026-01-27 14:30:00` |
| Action | `CHECK-OUT`, `CHECK-IN`, or `CONFLICT-RESOLUTION` | `CHECK-OUT` |
| File/Area | Path or scope | `docs/06-operations/`, `app/(tabs)/(home)/` |
| Agent | Consistent identifier | “Documentation Agent”, “Backend Agent” |
| Feature request | Link to Project item (when work is for a feature) | Project item URL |
| Duration | On check-in only | `15m`, `2h 30m` |

---

## Archive Policy

When `ACTIVE_WORK_LOG.md` exceeds **500 entries**:

1. **Move** entries older than 30 days to an archive file: `docs/08-status/active-work-archive/ACTIVE_WORK_LOG_ARCHIVE_YYYY-MM.md`.
2. **Keep** the last 30 days’ entries in `ACTIVE_WORK_LOG.md`.
3. **Do not delete** archive files; they are part of the historical record.
4. **Add** a one-line note at the top of `ACTIVE_WORK_LOG.md` after archiving: “Entries before YYYY-MM-DD archived to `active-work-archive/ACTIVE_WORK_LOG_ARCHIVE_YYYY-MM.md`.”

If the archive folder or naming differs for the project, document it in this section and in the ACTIVE_WORK_LOG file header.

---

## Output / Deliverables

| Deliverable | Location |
|-------------|----------|
| Current check-outs | `docs/08-status/ACTIVE_WORK.md` (table “Currently Active Work”) |
| Full history | `docs/08-status/ACTIVE_WORK_LOG.md` (append-only) |
| Archived log (when applicable) | `docs/08-status/active-work-archive/ACTIVE_WORK_LOG_ARCHIVE_YYYY-MM.md` |

---

## Integration with Other Processes

- **docs/README.md** (Process and behavior reference) instructs agents to check ACTIVE_WORK, then check out (add to ACTIVE_WORK + append ACTIVE_WORK_LOG) when starting; “Coordination” summarizes check-out/check-in and points here for full procedure.
- **docs/README.md:** Layer 8 links to ACTIVE_WORK and ACTIVE_WORK_LOG; Layer 6 links to this SOP so the chain **operation (SOP) → status files** is clear.
- **Feature work / examinations / migrations:** Any operation that edits tracked files or areas must follow this SOP so coordination is consistent. When work is for a specific feature, record **Feature request** (link to Project item with label `coordination:feature-request`) so agents know what they are working on. See [Coordination Model](../../01-orientation/COORDINATION_MODEL.md).

---

## Notes

- **ACTIVE_WORK.md** is for coordination only. It holds only currently checked-out items; remove entries when work is complete.
- **ACTIVE_WORK_LOG.md** is append-only. Never remove or edit prior rows.
- **Agent names** should be consistent (e.g. “Documentation Agent”, “Backend Agent”) to make the log readable.
- **File/area** should be specific enough to avoid collisions (e.g. `app/(tabs)/home.tsx` or `docs/06-operations/`).
- **Git** remains the source of truth for file contents; this SOP addresses coordination and conflict avoidance, not version control.

---

**Last Updated:** 2026-01-27
