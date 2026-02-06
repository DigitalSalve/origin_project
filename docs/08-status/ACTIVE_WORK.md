# Active Work Tracking

This document tracks files and areas currently being worked on by agents to prevent conflicts and coordinate work.

**Last Updated:** 2026-01-29

---

## How to Use

### Before Starting Work (Check-Out)

1. **Check this file** to see if the file/area you need is already checked out
2. **Add your entry** to "Currently Active Work" with:
   - File/area path
   - **Agent** identifier (e.g., "Documentation Agent", "Backend Agent", "Frontend Agent")
   - **Checked Out** timestamp (YYYY-MM-DD HH:MM:SS)
   - Brief description of work
   - Expected completion time
3. **Append to `ACTIVE_WORK_LOG.md`** with:
   - **Checked Out** timestamp (YYYY-MM-DD HH:MM:SS)
   - Action: `CHECK-OUT`
   - File/area path, agent, and description

### When Completing Work (Check-In)

1. **Remove your entry** from "Currently Active Work"
2. **Append to `ACTIVE_WORK_LOG.md`** with:
   - **Checked In** timestamp (YYYY-MM-DD HH:MM:SS)
   - Action: `CHECK-IN`
   - Same file/area, agent, and description
   - Duration (calculated from check-out time)

### If Conflict Detected

- If you see a file you need is already checked out:
  - Check the expected completion time
  - Check which agent has it checked out
  - Wait if reasonable, or
  - Contact the agent working on it
  - Document the conflict resolution in the log with timestamp

---

## Currently Active Work (Checked Out)

| File/Area | Agent | Checked Out | Description | Expected Completion |
|-----------|------|-------------|-------------|---------------------|
| *None currently* | - | - | - | - |

---

## Full History

**Complete check-out/check-in log:** See [`ACTIVE_WORK_LOG.md`](ACTIVE_WORK_LOG.md) for the full append-only history of all file access.

---

## Guidelines

- **Check before editing** - Always check this file before starting work on a file
- **Check-out immediately** - Add entry with timestamp when you start working, append to `ACTIVE_WORK_LOG.md`
- **Check-in immediately** - Remove entry from this file and append to `ACTIVE_WORK_LOG.md` when done
- **Use timestamps** - Format: `YYYY-MM-DD HH:MM:SS` (e.g., `2026-01-25 14:30:00`)
- **Identify agent clearly** - Use consistent agent names (e.g., "Documentation Agent", "Backend Agent")
- **Be specific** - Include full file paths or clear area descriptions
- **Set expectations** - Include expected completion time to help coordination
- **Calculate duration** - On check-in, calculate and record duration (e.g., "15m", "2h 30m")
- **Resolve conflicts** - If conflicts occur, document the resolution in the log
- **Log is append-only** - Never remove entries from `ACTIVE_WORK_LOG.md`
- **Full history** - Complete log is in `ACTIVE_WORK_LOG.md`
- **This file is for coordination only** - Only tracks currently checked-out files

---

## File Patterns to Track

- Code files: `backend/**/*.ts`, `app/**/*.tsx`
- Documentation: `docs/**/*.md`
- Database: `backend/db/migrations/*.sql`, `backend/db/schema.ts`
- Configuration: `package.json`, `app.json`, etc.

---

**Note:** This is a coordination tool. Git is still the source of truth for actual file changes.

**Additional Notes:**
- **Agent Identification:** Use consistent agent names to track who is working on what
- **Duration Tracking:** Helps identify patterns in work time and potential bottlenecks
- **Persistent Log:** Full audit trail is in `ACTIVE_WORK_LOG.md` (append-only, never deleted)
- **Archive Policy:** When log exceeds 500 entries, archive entries older than 30 days to `active-work-archive/ACTIVE_WORK_LOG_ARCHIVE_YYYY-MM.md`
- **This file is for coordination only** - Tracks currently checked-out files to prevent conflicts
