# Report Creation and Delivery SOP

**Purpose:** Standard Operating Procedure for creating, placing, logging, and delivering reports so every report is findable, traceable, and correctly linked from the documentation spine. Use when producing any report (examination, audit, verification, gap analysis, or one-off) so placement, naming, and delivery are consistent.

**Status:** Active

**Applies To:** All reports produced in or for this repository, whether by an SOP (Screen Examination, Backend Examination, User Flow Identification, etc.) or by ad hoc work (verification, gap analysis, documentation audit).

---

## Overview

This SOP defines:

1. **Where reports are placed** — SOP-specific folders vs. the general reports folder
2. **How to name reports** — Per [FILE_NAMING_CONVENTIONS.md](FILE_NAMING_CONVENTIONS.md)
3. **How to create and structure a report** — Required metadata, sections, and link to process/SOP
4. **How to deliver a report** — Log it, link it from the spine, and cross-reference related docs
5. **Lifecycle for reports in the general reports folder** — Active → Addressed → Archived

Use this SOP whenever you create a new report so the chain **operation → report → spine** stays intact.

---

## Where Reports Are Placed

**Primary flow:** Create Project items with label `coordination:report` for review. After review, optionally promote to `docs/08-status/reports/` for versioned snapshot. See [Coordination Model](../../01-orientation/COORDINATION_MODEL.md).

Reports also live under `docs/08-status/` in SOP-owned folders. Placement depends on **who produces the report**:

| Produced by | Report type | Location | Naming | Index |
|-------------|-------------|----------|--------|-------|
| **Screen Examination SOP** | Screen examination | `docs/08-status/screen-examination/` | `YYYY-MM-DD_SCREEN_EXAMINATION_REPORT.md` | Folder README; docs/README Layer 8 |
| **Backend Examination SOP** | Backend examination | `docs/08-status/backend-examination/` | `YYYY-MM-DD_BACKEND_EXAMINATION_REPORT.md` | [backend-examination/README](../08-status/backend-examination/README.md); docs/README Layer 8 |
| **User Flow Identification SOP** | User flow map (optional audit run) | `docs/08-status/user-flows/` | `YYYY-MM-DD_USER_FLOW_MAP_REPORT.md` | [user-flows/README](../08-status/user-flows/README.md); docs/README Layer 8 |
| **Other / one-off** | Verification, gap analysis, audit, etc. | Project item `coordination:report`; optionally `docs/08-status/reports/` | `YYYY-MM-DD_{REPORT_TYPE}_REPORT.md` | Project; [reports/README](../08-status/reports/README.md) |

**Rule:** If an SOP specifies an output path, use that path. For one-off reports, create Project item first; optionally add file to `reports/` after review.

---

## Prerequisites

Before creating a report:

1. **Read:** [FILE_NAMING_CONVENTIONS.md](FILE_NAMING_CONVENTIONS.md) — Dated reports use `YYYY-MM-DD_{REPORT_TYPE}_REPORT.md` and live under `docs/08-status/{report-category}/`.
2. **Decide placement:** Is this report produced by an SOP that has its own folder? If yes, use that folder and the naming in that SOP. If no, use `docs/08-status/reports/` and this SOP.
3. **If using `reports/`:** Skim [reports/README](../08-status/reports/README.md) and [REPORT_LOG](../08-status/reports/REPORT_LOG.md) for lifecycle and log format.

---

## Process

### Step 1: Choose Location and Name

- [ ] **Identify the source:** Which SOP or process produces this report? (Screen Examination, Backend Examination, User Flow Identification, or none.)
- [ ] **Set location:** Use the folder in the table above for that source; if none, use `docs/08-status/reports/`.
- [ ] **Set name:** Use the naming convention for that folder. For SOP-owned folders, the SOP defines it (e.g. `YYYY-MM-DD_SCREEN_EXAMINATION_REPORT.md`). For `reports/`, use `YYYY-MM-DD_{REPORT_TYPE}_REPORT.md` — e.g. `2026-01-27_VERIFICATION_REPORT.md`, `2026-01-27_GAP_ANALYSIS_REPORT.md`.

**Output:** Path and filename, e.g. `docs/08-status/reports/2026-01-27_VERIFICATION_REPORT.md`.

---

### Step 2: Create the Report File

- [ ] **Create the file** at the path from Step 1.
- [ ] **Include required header metadata:** Date (YYYY-MM-DD), examiner (e.g. AI Agent, name), repository/scope, and **Process:** link to the SOP or process that produced it (e.g. `Per docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md`).
- [ ] **Include required sections:** At least (1) Executive summary, (2) Main content (findings, inventory, or body), (3) Recommendations or next steps. Use the **report template** from the producing SOP when one exists (e.g. Screen Examination, Backend Examination, User Flow Identification).
- [ ] **Use clear headings** so the report can be scanned and linked to (e.g. "## 1. Executive Summary", "## 2. Findings").

**Output:** A complete report file with metadata and sections. No orphan report — every report states which process produced it.

---

### Step 3: Log and Index the Report

- [ ] **If the report is in an SOP-owned folder** (screen-examination, backend-examination, user-flows): Ensure the folder’s README or index (if any) describes how reports are named and that new reports are listed or linked from `docs/README.md` Layer 8 when they are the latest or notable. The producing SOP usually states “add to REPORT_LOG” or “list in folder README” — do that.
- [ ] **If the report is in `docs/08-status/reports/`:** Ensure Project item exists with `coordination:report`; link from spine when notable.

**Output:** Report is findable from the spine or Project.

---

### Step 4: Deliver — Link from Spine and Cross-Reference

- [ ] **Layer 8 (Status):** If this is a new report type or the latest run of an existing type, ensure `docs/README.md` Layer 8 has a link to the report or to the folder that contains it (e.g. “Reports under 08-status/screen-examination/”). For one-off reports, Project item is primary; optionally add to spine when promoted to `reports/`.
- [ ] **Related docs:** Link **to** the report from any doc that consumes or follows up on it — e.g. feature request (“Findings in 08-status/backend-examination/2026-01-27_BACKEND_EXAMINATION_REPORT.md”), Migration Executive Overview (“Screen examination: 08-status/screen-examination/…”), or IMPLEMENTATION_VERIFICATION (“See verification report 08-status/reports/…”).
- [ ] **Inside the report:** Link **from** the report to the SOP or process that produced it and to any key docs it references (e.g. architecture, behavior).

**Output:** Report is reachable from the docs spine and from related docs; the report itself points back to process and key references.

---

### Step 5: Lifecycle (Reports in `docs/08-status/reports/` Only)

For reports that live in `docs/08-status/reports/` (not in SOP-owned folders), follow this lifecycle:

- [ ] **Active:** New reports start as Active; they are being worked on or referenced.
- [ ] **Addressed:** When all issues or recommendations in the report have been acted on, update the report’s status section to “✅ Addressed” and add a short “Resolution Summary” (date, actions, verification). Update REPORT_LOG status to “Addressed.”
- [ ] **Archived:** When the report is no longer needed for day-to-day work, move the file to `docs/08-status/reports/archive/`, update REPORT_LOG with status “Archived,” archive date, and archive path. Do not delete; keep for history.

**Output:** REPORT_LOG and, when applicable, the report file itself reflect current status and location.

---

## Output / Deliverable

**Deliverable:** The report itself, plus logging/indexing and spine/context links as above.

**Locations (summary):**

| Report type | Location | Log / index |
|-------------|----------|------------|
| Screen examination | `08-status/screen-examination/YYYY-MM-DD_SCREEN_EXAMINATION_REPORT.md` | docs/README Layer 8; folder README if present |
| Backend examination | `08-status/backend-examination/YYYY-MM-DD_BACKEND_EXAMINATION_REPORT.md` | docs/README Layer 8; backend-examination/README |
| User flow map (audit) | `08-status/user-flows/YYYY-MM-DD_USER_FLOW_MAP_REPORT.md` | docs/README Layer 8; user-flows/README |
| Other (verification, gap, audit, etc.) | Project item `coordination:report`; optionally `08-status/reports/` | Project |

---

## Report Metadata Template (Minimum)

Every report should have at the top:

```markdown
# [Report Title]

**Date:** YYYY-MM-DD
**Examiner:** [AI Agent / name]
**Repository or scope:** [e.g. sourceapp, gop_app, "Member app only"]
**Process:** Per `docs/06-operations/[SOP_OR_PROCESS].md`

---
```

Then: Executive summary, main content, recommendations or next steps.

---

## Integration with Other Processes

- **FILE_NAMING_CONVENTIONS:** Report names and locations follow [FILE_NAMING_CONVENTIONS.md](FILE_NAMING_CONVENTIONS.md) (date-first for dated reports, `_REPORT` suffix, correct folder).
- **Screen / Backend / User Flow SOPs:** Those SOPs define their own output path and template; this SOP defines the **generic** creation, placement, logging, and delivery steps so nothing is dropped.
- **docs/README:** Layer 8 lists report folders and, where useful, latest reports; “operations → reports → spine” is kept intact.
- **Repo Integrity Check SOP:** Integrity checks include “reports and logs are linked and current”; running the integrity SOP helps catch missing log entries or broken report links.
- **reports/README:** [reports/README](../08-status/reports/README.md) describes the reports folder and lifecycle. Create Project items with `coordination:report`; optionally promote to folder after review.

---

## Notes

- **One report, one file:** Do not split a single logical report across multiple files; use one file and clear headings.
- **Always link to process:** Every report must state which SOP or process produced it so traceability is preserved.
- **Do not duplicate SOP-owned reports in `reports/`:** Screen, backend, and user-flow reports stay in their SOP-owned folders; they are not copied into `reports/`. Only reports that have no other home go in `reports/`.
- **Archiving is for `reports/` only:** SOP-owned folders may define their own retention; the Active → Addressed → Archived lifecycle and `reports/archive/` apply to reports in `docs/08-status/reports/`.

---

**Last Updated:** 2026-01-27
