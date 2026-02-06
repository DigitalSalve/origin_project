# Unified File Naming Conventions

**Purpose:** Single source of truth for how to name files and folders across this repository. Use when creating or renaming documentation, reports, feature requests, code assets, and migration artifacts so names are consistent and predictable.

**Status:** Active

**Applies To:** Documentation (`docs/`), reports, feature requests, status files, templates, and — when adopted by a project — source code and migrations in that project.

---

## Overview

This document defines:

1. **General rules** — Allowed characters, case, and separators
2. **Conventions by type** — SOPs, processes, reports, status files, feature requests, templates, and folders
3. **Examples and counter-examples** — Preferred vs discouraged names
4. **Process** — When creating a new file, check this document and apply the convention for that type

---

## General Rules

### Characters

- **Use:** Letters (A–Z, a–z), digits (0–9), hyphens (`-`), and underscores (`_`)
- **Avoid:** Spaces, special characters (e.g. `@`, `#`, `%`, `&`), and emoji in file and folder names
- **Extensions:** Use lowercase (e.g. `.md`, `.ts`, `.tsx`, `.sql`)

### Case and separators

- **Documentation and status files:** `UPPER_SNAKE_CASE` (e.g. `ACTIVE_WORK.md`, `SCREEN_EXAMINATION_SOP.md`)
- **Reports with dates:** `YYYY-MM-DD_DESCRIPTIVE_SUFFIX.ext` (e.g. `2026-01-27_SCREEN_EXAMINATION_REPORT.md`)
- **Folders:** `lowercase-with-hyphens` for multi-word names (e.g. `backend-examination`, `feature-requests`, `screen-examination`); single lowercase words when one word (e.g. `reports`, `qa`)
- **Code files (when project adopts this):** Prefer project-specific convention (e.g. PascalCase for React components, camelCase for utils, kebab-case for routes); document in project canon if it differs from docs

### Uniqueness and clarity

- **Globally unique within scope:** No two files in the same directory should share the same name
- **Descriptive:** The name should indicate content or purpose without opening the file (e.g. `MIGRATION_PROCESS.md` not `PROCESS.md`)
- **Suffixes for type:** Use consistent suffixes for document type (e.g. `_SOP`, `_REPORT`, `_TODO`, `_PROCESS`) so sorting and search behave predictably

---

## Conventions by Type

### Standard Operating Procedures (SOPs)

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{DESCRIPTIVE_NAME}_SOP.md` | `SCREEN_EXAMINATION_SOP.md`, `BACKEND_EXAMINATION_SOP.md` |
| Case | UPPER_SNAKE_CASE | — |
| Location | `docs/06-operations/` | — |
| Suffix | `_SOP` | Required |

**Preferred:** `SCREEN_EXAMINATION_SOP.md`, `BACKEND_EXAMINATION_SOP.md`  
**Avoid:** `ScreenExaminationSOP.md`, `screen-examination-sop.md`, `SOP_Screen_Examination.md`

---

### Process Documents

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{DESCRIPTIVE_NAME}_PROCESS.md` or `{DESCRIPTIVE_NAME}_PROCESS.md` | `MIGRATION_PROCESS.md`, `DOCUMENTATION_PROCESS.md` |
| Case | UPPER_SNAKE_CASE | — |
| Location | `docs/06-operations/` (or as specified by project) | — |
| Suffix | `_PROCESS` | Recommended when the doc defines a process rather than a one-off procedure |

**Preferred:** `MIGRATION_PROCESS.md`, `DOCUMENTATION_PROCESS.md`  
**Avoid:** `MigrationProcess.md`, `migration-process.md`

---

### Dated Reports (Examination, Audit, Backend Reports)

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `YYYY-MM-DD_{REPORT_TYPE}_REPORT.md` | `2026-01-27_SCREEN_EXAMINATION_REPORT.md`, `2026-01-15_BACKEND_EXAMINATION_REPORT.md` |
| Date | ISO date first (YYYY-MM-DD), underscore | Ensures chronological sort |
| Case | UPPER_SNAKE_CASE for the remainder | — |
| Suffix | `_REPORT` | Required |
| Location | `docs/08-status/{report-category}/` | e.g. `screen-examination/`, `backend-examination/` |

**Preferred:** `2026-01-27_SCREEN_EXAMINATION_REPORT.md`, `2026-02-01_BACKEND_EXAMINATION_REPORT.md`  
**Avoid:** `Screen_Examination_2026-01-27.md`, `report-2026-01-27.md`, `2026/01/27_REPORT.md`

---

### Status and Coordination Files

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{PURPOSE}_TODO.md`, `{PURPOSE}_LOG.md`, `ACTIVE_WORK.md`, `CURRENT_STATUS.md`, etc. | `FEATURE_REQUEST_TODO.md`, `ACTIVE_WORK_LOG.md`, `QA_TODO.md`, `QA_WORK_LOG.md` |
| Case | UPPER_SNAKE_CASE | — |
| Location | `docs/08-status/` or subdirs (e.g. `feature-requests/`, `qa/`, `reports/`) | — |

**Preferred:** `ACTIVE_WORK.md`, `ACTIVE_WORK_LOG.md`, `INTEGRITY_CHECK_LOG.md`, `FEATURE_REQUEST_TODO.md`, `CURRENT_STATUS.md`, `IMPLEMENTATION_VERIFICATION.md`, `MIGRATION_EXECUTIVE_OVERVIEW.md`  
**Avoid:** `activeWork.md`, `feature-request-todo.md`, `Todo.md`

**Integrity Check Log:** `docs/08-status/INTEGRITY_CHECK_LOG.md` — Append-only log of daily repo integrity checks per [Repo Integrity Check SOP](REPO_INTEGRITY_CHECK_SOP.md). One row per run; date (YYYY-MM-DD), runner, result (Pass/Fail), summary.

---

### Feature Requests and Plans

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{TOPIC}_{DESCRIPTOR}.md` or `{FEATURE_NAME}_PLAN.md` / `_CHANGES.md` / `_QUICK_REFERENCE.md` | `FLUTTER_CONVERSION_PLAN.md`, `FLUTTER_CONVERSION_PROCESS_CHANGES.md`, `FLUTTER_CONVERSION_QUICK_REFERENCE.md` |
| Case | UPPER_SNAKE_CASE | — |
| Location | `docs/08-status/feature-requests/` | — |
| Suffix | `_PLAN`, `_CHANGES`, `_QUICK_REFERENCE`, or descriptive (e.g. `_TODO`) as needed | — |

**Preferred:** `FLUTTER_CONVERSION_PLAN.md`, `AUTH_REFACTOR_PLAN.md`  
**Avoid:** `flutter-conversion-plan.md`, `FlutterConversionPlan.md`

---

### Templates

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{TEMPLATE_TYPE}_TEMPLATE.md` or `{PURPOSE}_NOTE.md` (for feature notes) | `SOP_TEMPLATE.md`, `FEATURE_NOTE.md` |
| Case | UPPER_SNAKE_CASE | — |
| Location | `docs/07-guides/templates/` | — |
| Suffix | `_TEMPLATE` or `_NOTE` (or other type) | Keeps templates easy to find |

**Preferred:** `SOP_TEMPLATE.md`, `FEATURE_NOTE.md`  
**Avoid:** `SopTemplate.md`, `feature-note.md`

---

### Orientation and Canon

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{TOPIC}.md` or `{DESCRIPTIVE_NAME}.md` | `GETTING_STARTED.md`, `GUIDELINES.md`, `README.md` |
| Case | UPPER_SNAKE_CASE for multi-word; `README` remains uppercase by convention | — |
| Location | `docs/01-orientation/`, `docs/02-canon/`, or repo/docs root as appropriate | — |

**Preferred:** `GETTING_STARTED.md`, `GUIDELINES.md`, `README.md`, `CHATGPT_PROJECT_DOCUMENTATION_LIST.md`  
**Avoid:** `GettingStarted.md`, `getting-started.md`

---

### Conventions and Operations Guides

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `{TOPIC}_CONVENTIONS.md` or `{TOPIC}_SOP.md` / `_PROCESS.md` | `FILE_NAMING_CONVENTIONS.md` |
| Case | UPPER_SNAKE_CASE | — |
| Location | `docs/06-operations/` | — |

**Preferred:** `FILE_NAMING_CONVENTIONS.md`  
**Avoid:** `file-naming-conventions.md`, `FileNamingConventions.md`

---

### Folders

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `lowercase-with-hyphens` for multi-word; single lowercase word when one word | `backend-examination`, `screen-examination`, `feature-requests`, `reports`, `qa` |
| Avoid | underscores, UPPER_SNAKE, spaces | — |

**Preferred:** `backend-examination/`, `screen-examination/`, `feature-requests/`, `reports/`, `qa/`  
**Avoid:** `backend_examination/`, `BackendExamination/`, `backend examination/`

**Operations subfolders:** `docs/06-operations/legal/` and `docs/06-operations/content/` — single-word lowercase. Files inside use `UPPER_SNAKE_CASE.md` (e.g. `TERMS_OF_SERVICE.md`, `PRIVACY_POLICY.md`, `HOMEPAGE.md`). See [Legal Documents SOP](LEGAL_DOCUMENTS_SOP.md) and [Content Documents SOP](CONTENT_DOCUMENTS_SOP.md) for naming and structure.

---

### Migrations (When Project Adopts)

| Rule | Convention | Example |
|------|------------|---------|
| Pattern | `YYYYMMDD_descriptive_snake_case.sql` or `.ts` per project | `20250220_add_deal_redemption_limits.sql`, `20250322_create_users_and_permissions.sql` |
| Date | YYYYMMDD (no separators), underscore, then descriptive snake_case | Chronological sort, readability |
| Location | Project-defined (e.g. `backend/db/migrations/`) | — |

**Preferred:** `20250220_add_deal_redemption_limits.sql`  
**Avoid:** `2025-02-20_add_deal_redemption_limits.sql`, `add_deal_redemption_limits.sql`

---

## Quick Reference Table

| Type | Pattern | Example |
|------|---------|---------|
| SOP | `{NAME}_SOP.md` | `SCREEN_EXAMINATION_SOP.md` |
| Process | `{NAME}_PROCESS.md` | `MIGRATION_PROCESS.md` |
| Dated report | `YYYY-MM-DD_{TYPE}_REPORT.md` | `2026-01-27_SCREEN_EXAMINATION_REPORT.md` |
| Status / coordination | `{PURPOSE}.md` (UPPER_SNAKE) | `ACTIVE_WORK.md`, `FEATURE_REQUEST_TODO.md` |
| Feature request / plan | `{TOPIC}_{DESCRIPTOR}.md` | `FLUTTER_CONVERSION_PLAN.md` |
| Template | `{TYPE}_TEMPLATE.md` or `{PURPOSE}_NOTE.md` | `SOP_TEMPLATE.md`, `FEATURE_NOTE.md` |
| Conventions / operations | `{TOPIC}_CONVENTIONS.md` or `_SOP` / `_PROCESS` | `FILE_NAMING_CONVENTIONS.md` |
| Folder | `lowercase-with-hyphens` or `lowercase` | `backend-examination/`, `qa/` |
| Migration | `YYYYMMDD_descriptive_name.sql` | `20250220_add_deal_redemption_limits.sql` |

---

## Process: When Creating a New File

1. **Identify the type** of file (SOP, process, report, status, feature request, template, folder, migration).
2. **Apply the convention** from this document for that type (see **Conventions by Type** or **Quick Reference Table**).
3. **Ensure uniqueness** in the target directory.
4. **Add the file to the docs spine** — If it is documentation, add a link in `docs/README.md` (or the relevant layer) so it is discoverable.
5. **Do not rename** existing files solely for style unless the project has decided to adopt this doc retroactively; for new files, use these conventions from the start.

---

## Exceptions

- **`README.md`** — Keep the uppercase `README`; this is a widely used convention.
- **Project-specific overrides** — When a project (e.g. gop_app, or a codebase under sourceapp) declares different naming in its canon or style guide, that project’s rules take precedence for that project’s files; this doc still applies to shared docs in `docs/` unless that project explicitly opts out.
- **Third-party or generated files** — Naming of vendored or generated assets may follow external conventions; document any such exception in project canon or in this section when they are checked in.

---

## Maintaining This Document

- **Updates:** Change this file when the project adopts a new document type or a new convention; keep the **Quick Reference Table** in sync with **Conventions by Type**.
- **Discovery:** This document is linked from `docs/README.md` under Layer 6 (Operations). New SOPs or process docs can reference it for consistency.
- **Scope expansion:** If conventions are later defined for source code (components, utils, routes, tests), add a **Code** subsection under **Conventions by Type** and reference the project or tech stack that uses it.

---

**Last Updated:** 2026-01-27
