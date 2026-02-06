# Infrastructure Template Sync — SOP

**Purpose:** Standard Operating Procedure for syncing core infrastructure file updates from active projects to the origin_project template (`~/Github/origin_project/`). Ensures the template stays current when infrastructure is improved or added in any project.

**Status:** Active  
**Applies To:** Any infrastructure file changes in SourceApp (or other projects using this infrastructure base)  
**Created:** 2026-01-28

---

## Overview

This SOP ensures that:

1. **Infrastructure improvements propagate** — When infrastructure is updated in one project, the update flows to the origin_project template
2. **Template stays current** — The origin_project template always reflects the latest infrastructure patterns
3. **New projects benefit** — Projects started from the template get latest improvements automatically
4. **Consistency maintained** — All projects can sync infrastructure updates from the template

**The origin_project template at `~/Github/origin_project/` is the single source of truth for reusable infrastructure.**

---

## Prerequisites

Before syncing infrastructure:

1. **Understand what is infrastructure:**
   - Read: `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md`
   - Verify: File is marked as reusable in inventory
   - Check: File has no project-specific hardcoded content

2. **Verify change is infrastructure-level:**
   - ✅ Process improvements (SOPs, workflows)
   - ✅ Template updates (feature notes, SOPs)
   - ✅ Infrastructure patterns (auth, deployment, architecture)
   - ❌ Project-specific content (canon, architecture, behavior)
   - ❌ Project-specific SOPs (e.g., SOURCEAPP_*)

3. **Have access to origin_project template:**
   - Location: `~/Github/origin_project/`
   - Ensure folder exists and is up to date

---

## Process

### Step 1: Identify Infrastructure Change

**When a file in infrastructure layers is modified:**

- [ ] **Check if file is infrastructure** per `CORE_INFRASTRUCTURE_INVENTORY.md`:
  - Layer 1: Orientation files (except PROJECT_IDENTITY contents)
  - Layer 6: Operations (SOPs, processes, conventions, management, usage)
  - Layer 7: Guides (INFRASTRUCTURE_*.md, templates)
  - Layer 8: Structure and formats (not contents)

- [ ] **Verify no project-specific content:**
  - No hardcoded project names (use "this project" or "see PROJECT_IDENTITY")
  - No project-specific examples that aren't marked as examples
  - No domain names or URLs specific to one project
  - No business logic specific to one domain

**Output:** Confirmed infrastructure file that needs syncing

---

### Step 2: Sync to origin_project Template

**Copy updated file to origin_project template:**

- [ ] **Navigate to source project:**
  ```bash
  cd ~/Github/project_sourceapp/sourceapp
  ```

- [ ] **Copy file to origin_project template:**
  ```bash
  # For infrastructure file in Layer 1, 6, or 7:
  cp [changed-file-path] ~/Github/origin_project/[same-path]
  
  # Example:
  cp docs/06-operations/processes/DOCUMENTATION_PROCESS.md \
     ~/Github/origin_project/docs/06-operations/processes/DOCUMENTATION_PROCESS.md
  ```

- [ ] **Verify copy successful:**
  ```bash
  diff [source-file] ~/Github/origin_project/[same-path]
  # Should show no differences
  ```

**Output:** File synced to origin_project template

---

### Step 3: Update Infrastructure Inventory

**If new infrastructure file added:**

- [ ] **Add to CORE_INFRASTRUCTURE_INVENTORY.md:**
  ```bash
  cd ~/Github/origin_project
  edit docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md
  ```

- [ ] **Add entry in appropriate category:**
  - File path
  - Purpose
  - Reusability indicator
  - When to copy guidance

- [ ] **Update category count** in Quick Reference table

- [ ] **Add to "Recent Additions"** section at bottom:
  ```markdown
  ### YYYY-MM-DD:
  - ✅ Added: [FILE_NAME].md (Layer X)
    - [Brief description]
    - [Benefit/purpose]
  ```

**Output:** Inventory reflects new infrastructure

---

### Step 4: Sync to Other Active Projects (Optional)

**If multiple projects use this infrastructure:**

- [ ] **Identify other projects** using this infrastructure
- [ ] **For each project, copy infrastructure file:**
  ```bash
  # Copy from origin_project to other projects
  cp ~/Github/origin_project/[infrastructure-file] \
     ~/Github/other-project/[same-path]
  ```

- [ ] **Verify no project-specific overrides** are lost
- [ ] **Test in other project** (if critical infrastructure)

**Output:** All projects have latest infrastructure

---

### Step 5: Document the Sync

**Record the infrastructure update:**

- [ ] **Add entry to origin_project template's version log:**
  ```bash
  cd ~/Github/origin_project
  echo "YYYY-MM-DD: Updated [FILE_NAME] - [reason]" >> INFRASTRUCTURE_VERSION_LOG.md
  ```

- [ ] **If major change, bump version** in MANIFEST.md:
  ```markdown
  ### Version 1.1 (YYYY-MM-DD)
  - Updated: [WHAT_CHANGED]
  - Added: [NEW_FILES]
  - Improved: [ENHANCEMENTS]
  ```

- [ ] **Optionally commit to Git** (if origin_project is version controlled):
  ```bash
  cd ~/Github/origin_project
  git add .
  git commit -m "Infrastructure update: [description]"
  ```

**Output:** Infrastructure sync documented

---

## When Creating New Project-Agnostic Infrastructure

**When you add a new project-agnostic SOP, process, or infrastructure doc (e.g. new `*_SOP.md` in Layer 6, new `INFRASTRUCTURE_*.md` in Layer 7):**

1. **Add to CORE_INFRASTRUCTURE_INVENTORY** — In `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md`, add an entry for the new file (path, purpose, reusability). Update category counts if needed.
2. **Copy to origin_project** — Copy the new file to `~/Github/origin_project/[same-path]`. Create parent directories in origin_project if they do not exist.
3. **Log the sync** — Append an entry to `~/Github/origin_project/INFRASTRUCTURE_VERSION_LOG.md` (date, file added, brief reason).
4. **Link in docs/README.md** — Ensure the new doc is linked in the correct layer in `docs/README.md` (and in `docs/06-operations/README.md` if under Layer 6).

**Do this immediately** when creating new project-agnostic documentation; do not defer to the monthly audit.

---

## Verify origin_project Has All Inventory Files

**Periodically (e.g. during audit or before release) check that the origin_project template is complete:**

- [ ] **Open CORE_INFRASTRUCTURE_INVENTORY.md** and list every file path marked as reusable.
- [ ] **For each path,** verify the file exists at `~/Github/origin_project/[path]`. If it is missing, copy from this project to origin_project and log the sync.
- [ ] **Scan this project** for new infrastructure not yet in the inventory (e.g. `docs/06-operations/**/*_SOP.md`, `docs/07-guides/INFRASTRUCTURE_*.md`). Any project-agnostic file that belongs in the template must be added to the inventory and copied to origin_project.

**Output:** origin_project directory contains all files in the inventory; any new infrastructure has been added to inventory and synced to origin_project.

---

## When to Sync

### ✅ Always Sync:

1. **New SOP created** (Layer 6)
2. **Infrastructure pattern added** (Layer 7, INFRASTRUCTURE_*.md)
3. **Template updated** (Layer 7, templates/)
4. **Process improved** (any SOP modification)
5. **AI behavior rules improved** (AI.md)
6. **Documentation spine updated** (docs/README.md structure)

### ⚠️ Review Before Syncing:

1. **File has project examples** (ensure marked as examples)
2. **File references specific project** (change to "this project")
3. **SOP has project context** (extract generic process)

### ❌ Never Sync:

1. **Project-specific SOPs** (SOURCEAPP_*.md)
2. **Canon/Architecture/Behavior contents** (Layers 2-5)
3. **Feature requests** (Layer 8 contents)
4. **Reports or logs** (Layer 8 contents)
5. **Legal/content document contents** (Layer 6, contents only)

---

## Sync Checklist Template

Use this checklist when syncing:

```markdown
## Infrastructure Sync Checklist

Date: YYYY-MM-DD
File(s): [list]
Project: [source project]
Reason: [why changed]

- [ ] File is in infrastructure inventory
- [ ] No project-specific content (checked)
- [ ] Copied to ~/Github/origin_project/[path]
- [ ] Verified with diff (no errors)
- [ ] Updated CORE_INFRASTRUCTURE_INVENTORY.md (if new)
- [ ] Added to Recent Additions
- [ ] Updated category count (if new)
- [ ] Synced to other projects: [list or N/A]
- [ ] Documented sync in version log
- [ ] Committed to Git (if applicable)
- [ ] (Periodic) Verified origin_project has all files in CORE_INFRASTRUCTURE_INVENTORY; synced any missing

Synced by: [agent/person]
```

---

## Examples

### Example 1: New Infrastructure Pattern Added

```bash
# Created new pattern in SourceApp
File: docs/07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md

# Sync to origin_project
cd ~/Github/project_sourceapp/sourceapp
cp docs/07-guides/INFRASTRUCTURE_OPTIMAL_FRONTEND_BACKEND_ARCHITECTURE.md \
   ~/Github/origin_project/docs/07-guides/

# Update inventory
cd ~/Github/origin_project
edit docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md
# Add to Category 10: Infrastructure Guides

# Document
echo "2026-01-28: Added optimal architecture pattern" >> INFRASTRUCTURE_VERSION_LOG.md
```

---

### Example 2: SOP Process Improved

```bash
# Improved SOP in SourceApp
File: docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md
Change: Added new checklist item for error handling

# Sync to origin_project
cd ~/Github/project_sourceapp/sourceapp
cp docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md \
   ~/Github/origin_project/docs/06-operations/examination/

# Verify
diff docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md \
     ~/Github/origin_project/docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md

# Document
cd ~/Github/origin_project
echo "YYYY-MM-DD: Improved SCREEN_EXAMINATION_SOP - added error handling check" >> INFRASTRUCTURE_VERSION_LOG.md
```

---

## Notes

- **Sync direction:** Active projects → origin_project template → other projects
- **origin_project is source of truth:** The template is authoritative for infrastructure
- **Project-specific stays separate:** Never sync project-specific content
- **Bidirectional:** Projects can also pull updates from origin_project
- **Version control recommended:** Keep origin_project in Git for history

---

## Integration with Other Processes

- **CORE_INFRASTRUCTURE_INVENTORY.md:** Defines what is infrastructure (must be checked first)
- **BOOTSTRAP_NEW_PROJECT.md:** Uses origin_project template to start new projects
- **DOCUMENTATION_PROCESS.md:** When infrastructure is documented as part of features
- **REPO_INTEGRITY_CHECK_SOP.md:** Can check if project infrastructure is out of sync with origin_project

---

## Related Documentation

- [Core Infrastructure Inventory](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) — What qualifies as infrastructure
- [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md) — How new projects use the template
- [Infrastructure Template Audit SOP](INFRASTRUCTURE_TEMPLATE_AUDIT_SOP.md) — How to check for sync drift

---

**Last Updated:** 2026-01-28
