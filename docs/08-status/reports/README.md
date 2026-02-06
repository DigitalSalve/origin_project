# Documentation Reports

**New reports:** Create Project items with label `coordination:report` for review in collaboration. See [Coordination Model](../../01-orientation/COORDINATION_MODEL.md). After review, optionally promote to this folder for versioned snapshot.

---

This directory contains time-bound reports that are **not** produced by a specific SOP (e.g. verification, gap analysis, audit, consolidation, implementation status, architecture summary, infrastructure template completion). Reports produced by Screen Examination, Backend Examination, or User Flow Identification go in their SOP-owned folders under `08-status/`. **How to create and deliver any report:** [Report Creation and Delivery SOP](../../06-operations/conventions/REPORT_CREATION_AND_DELIVERY_SOP.md).

**Rule:** Status and deliverable reports (e.g. "what we created," "mission complete") belong here under `docs/08-status/reports/`, not in the repository root. Repo root should hold only `AI.md`, `README.md`, and code/config; all documentation lives under `docs/`.

## Structure

```
reports/
├── README.md (this file)
└── [REPORT_NAME]_YYYY-MM-DD.md (optional: promoted snapshots after Project review)
```

## Report Lifecycle

1. **Created** - Create Project item with label `coordination:report`; optionally create file: `REPORT_NAME_YYYY-MM-DD.md`
2. **Active** - Report is being worked on or referenced in Project
3. **Addressed** - All issues in report have been resolved
4. **Archived** - Optionally move to `archive/` subdirectory; close Project item

## Creating a New Report

1. Create Project item with label `coordination:report`; optionally create the report file: `REPORT_NAME_YYYY-MM-DD.md`
2. Include in report:
   - Executive summary
   - Findings/issues
   - Recommendations
   - Resolution tracking section

## Archiving a Report

When a report is fully addressed:

1. Update the report's status section to "✅ Addressed"
2. Add "Resolution Summary" section with:
   - Date addressed
   - Actions taken
   - Verification that issues are resolved
3. Optionally move report to `archive/` directory
4. Close the Project item

## Report Types

- **Verification Reports** - Verify documentation accuracy against codebase
- **Gap Analysis** - Identify missing documentation
- **Audit Reports** - Review documentation quality and completeness
- **Consolidation Reports** - Track documentation reorganization efforts
- **Implementation Status** - Track feature implementation progress

## Guidelines

- **Naming:** Always include date in filename: `REPORT_NAME_YYYY-MM-DD.md`
- **Status Tracking:** Track status in Project item
- **Resolution:** Document how issues were addressed before archiving
- **Retention:** Keep archived reports indefinitely for historical reference
- **Cross-References:** Link to relevant documentation in `03-architecture/` or other layers
