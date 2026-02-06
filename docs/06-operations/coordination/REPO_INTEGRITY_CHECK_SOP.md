# Repo Integrity Check — SOP

**Purpose:** Standard Operating Procedure to verify documentation and repo integrity (flow, links, operations–reports correlation). The check is run **at least once per calendar day**. If `docs/08-status/INTEGRITY_CHECK_LOG.md` does not show an entry for **today** (YYYY-MM-DD), run this SOP **before performing the next operation** (e.g. feature work, new docs, examination, migration step).

**Status:** Active

**Applies To:** AI agents and humans performing documentation, feature, or operational work in this repository. The “run before next op if no check today” rule is enforced via `AI.md` and `docs/README.md`.

---

## Overview

This SOP ensures that:

1. **Daily cadence** — A repo integrity check is run at least once per calendar day and recorded in the Integrity Check Log.
2. **Gate before next operation** — If the log has no entry for today, this SOP is run *before* the next operation (feature implementation, new doc, examination, migration, etc.).
3. **Consistent checks** — Each run verifies the same set of integrity conditions (spine, layers, links, operations–reports chain).
4. **Append-only log** — Every run is recorded in `docs/08-status/INTEGRITY_CHECK_LOG.md` with date, runner, result (Pass/Fail), and optional summary. Entries are never removed.

---

## Prerequisites

Before running the repo integrity check:

1. **Read:** `AI.md`, `docs/README.md`, and this SOP.
2. **Locate the log:** `docs/08-status/INTEGRITY_CHECK_LOG.md`. Use **today’s date** in YYYY-MM-DD (repository/server date or agreed “operational date”).
3. **Rule:** If the log already contains an entry for today, you may skip the check for this run (optional re-run is allowed). If it does *not* contain an entry for today, you **must** run the full check and append an entry before doing any other operation.

---

## Process

### Step 0: Decide Whether to Run

- [ ] **Open** `docs/08-status/INTEGRITY_CHECK_LOG.md`.
- [ ] **Check** whether there is an entry for **today** (YYYY-MM-DD) in the log.
- [ ] **If yes:** No need to run this SOP for today unless you are doing an explicit re-check. Proceed with your intended operation.
- [ ] **If no:** You **must** complete Steps 1–5 below and append an entry to the log before performing any other operation. Do not skip.

**Output:** Decision “already checked today” or “must run full check now.”

---

### Step 1: Verify AI.md → docs/README Flow

- [ ] **AI.md** exists at repository root and specifies `docs/README.md` as the documentation spine.
- [ ] **AI.md** defines required reading order (1–8) and points to `docs/01-orientation/` through `docs/08-status/`.
- [ ] **AI.md** “Key files and routing” or “Documentation and repo integrity” describes the flow (AI.md → docs/README → layer) and the rule that operations/reports are discoverable via docs/README.
- [ ] **docs/README.md** exists and has Layer 1–8 sections with working links (no 404s or wrong paths).

**What to look for:** Broken or missing links in AI.md to docs/README or GETTING_STARTED; missing or empty docs/README.

**Output:** Pass if spine and reading order are intact; Fail if any of the above is missing or broken.

---

### Step 2: Verify docs/README Layer 6 (Operations)

- [ ] **Layer 6** in docs/README lists all operations that exist in `docs/06-operations/` and its subfolders: **processes/** (Documentation Process, Migration Process, Scope of Work SOP), **examination/** (Screen, Backend, User Flow Identification SOPs), **coordination/** (Active Work, Repo Integrity Check, Implementation Verification SOPs), **conventions/** (File Naming, Report Creation and Delivery SOP), **management/** (Integration Platforms, Legal, Content SOPs), **usage/** (behavior-doc usage SOPs), plus **legal/** and **content/** folders.
- [ ] Every linked operations item resolves (file or folder exists). Links use paths like `06-operations/processes/DOCUMENTATION_PROCESS.md`, `06-operations/coordination/REPO_INTEGRITY_CHECK_SOP.md`, etc.
- [ ] No SOP or process file in `docs/06-operations/` or its subfolders is missing from docs/README Layer 6 (no orphaned operations).

**What to look for:** New SOPs or process docs added under `docs/06-operations/` (or a subfolder) but not linked in docs/README; broken links to 06-operations.

**Output:** Pass if all operations are linked and resolvable; Fail if something is missing or broken.

---

### Step 3: Verify docs/README Layer 8 (Reports and Status)

- [ ] **Layer 8** in docs/README lists status files (e.g. Current Status, Migration Executive Overview, Active Work, Active Work Log, Implementation Verification), feature requests, QA, report locations (e.g. screen-examination, backend-examination), and **Integrity Check Log**.
- [ ] Every linked Layer 8 item resolves (file or folder exists).
- [ ] Report output paths match what the governing SOPs specify (e.g. Screen Examination → `08-status/screen-examination/`, Backend Examination → `08-status/backend-examination/`).

**What to look for:** New report folders or logs added under `docs/08-status/` but not linked in docs/README; broken links; mismatch between SOP output location and docs/README.

**Output:** Pass if all status/report entries are linked and resolvable and match SOP output locations; Fail if something is missing, broken, or inconsistent.

---

### Step 4: Verify No Orphaned Docs (Spot Check)

- [ ] **Spot check:** A representative doc in each layer (e.g. one from 02-canon, 03-architecture, 04-behavior, 05-integrations, 06-operations, 08-status) is linked from docs/README. No need to enumerate every file; ensure the layer section links to real files/folders and that at least one linked item per layer exists.
- [ ] **Integrity Check Log** exists at `docs/08-status/INTEGRITY_CHECK_LOG.md` and is linked from docs/README Layer 8.

**What to look for:** Entire layers or key files (e.g. INTEGRITY_CHECK_LOG) not linked from docs/README.

**Output:** Pass if spot check shows layers and key files are reachable from docs/README; Fail if a key piece is missing.

---

### Step 5: Record Run in Integrity Check Log

- [ ] **Open** `docs/08-status/INTEGRITY_CHECK_LOG.md`.
- [ ] **Append** one new row: `| YYYY-MM-DD | HH:MM | Runner (e.g. AI Agent, name) | Pass or Fail | One-line summary (optional) |`.
- [ ] **Use today’s date** (YYYY-MM-DD) and the time of the run. Do not backdate. Do not remove or edit existing entries.
- [ ] If the run **Failed**, set Result = Fail and put a short reason in the summary (e.g. “Layer 6 missing link to X”). Fixes may be done in the same or a follow-up change; the log still records the Fail so that re-runs can confirm Pass after fixes.

**Output:** INTEGRITY_CHECK_LOG has exactly one new append for this run. No other edits to the log.

---

## Output / Deliverable

**Deliverable:** Integrity Check Log entry.

**Location:** `docs/08-status/INTEGRITY_CHECK_LOG.md`

**When to create:** At the end of every run of this SOP (i.e. whenever Step 0 determined “must run full check now”). One row per run; one run per day minimum when performing operations.

---

## Integration with Other Processes

- **AI.md:** “Before making changes” instructs agents to check INTEGRITY_CHECK_LOG for today; if no entry for today, run this SOP before the next operation.
- **docs/README.md:** “Documentation and repo integrity” and Layer 8 describe the daily-check rule and link to this SOP and the log.
- **Documentation Process:** Keeping docs in sync (e.g. adding new SOPs to docs/README) supports integrity; a failed check often indicates a missing link or new doc not yet registered in docs/README.
- **No other operation should be performed** until either (a) the log shows an entry for today, or (b) this SOP has been run and an entry appended for today.

---

## Notes

- **Calendar day** is defined in repository/server timezone or an agreed “operational date” for the repo. When in doubt, use UTC date for the log.
- **Runner** can be “AI Agent,” a named user, or a script identifier. Keep it short.
- **Pass/Fail** is per Steps 1–4. If any step fails, the run is Fail; fix issues and re-run to get Pass, then append the new run.
- **Append-only:** Never delete or alter prior log rows. Archives or rotation (if any) are defined in the log file itself.
- **Frequency:** Minimum once per day when any operation is performed. Additional runs in the same day are allowed and should each append one row.

---

**Last Updated:** 2026-01-27
