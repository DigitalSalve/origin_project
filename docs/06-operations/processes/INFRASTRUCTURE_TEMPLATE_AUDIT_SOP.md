# Infrastructure Template Audit — SOP

**Purpose:** Standard Operating Procedure for auditing whether the blank infrastructure template (`~/Github/blank/`) is out of sync with active projects and needs updates. Run periodically to ensure template stays current.

**Status:** Active  
**Applies To:** Periodic maintenance (recommended: monthly or after major infrastructure changes)  
**Created:** 2026-01-28

---

## Overview

This SOP ensures that:

1. **Detect drift** — Identify when active projects have infrastructure improvements not yet in the template
2. **Prioritize updates** — Determine which infrastructure changes should be synced
3. **Maintain template** — Keep blank template as the authoritative source of infrastructure
4. **Enable new projects** — Ensure new projects start with latest infrastructure

**Run frequency:** Monthly, or after any significant infrastructure change in an active project.

---

## Prerequisites

Before auditing:

1. **Access to:**
   - Blank template: `~/Github/blank/`
   - Active project: `~/Github/project_sourceapp/sourceapp/`

2. **Read documentation:**
   - `~/Github/blank/docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md` — What is infrastructure
   - `docs/06-operations/processes/INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md` — How to sync

3. **Understand infrastructure categories:**
   - Layer 1: Orientation (navigation, bootstrap)
   - Layer 6: Operations (SOPs, processes)
   - Layer 7: Guides (infrastructure patterns, templates)
   - Layer 8: Structure (formats, not contents)

---

## Process

### Step 1: Compare Infrastructure Files

**For each infrastructure file in CORE_INFRASTRUCTURE_INVENTORY:**

- [ ] **Run diff between project and blank:**
  ```bash
  # Compare SourceApp to blank
  diff ~/Github/project_sourceapp/sourceapp/[infrastructure-file] \
       ~/Github/blank/[same-path]
  ```

- [ ] **Record differences:**
  - File path
  - Which project is ahead
  - Nature of change (content added, process improved, etc.)
  - Date last modified in each location

**Output:** List of infrastructure files with differences

---

### Step 2: Classify Differences

**For each difference found:**

- [ ] **Determine if change is infrastructure-appropriate:**

| Change Type | Action |
|------------|--------|
| **Process improvement** | ✅ Sync to blank |
| **New SOP added** | ✅ Sync to blank |
| **Infrastructure pattern added** | ✅ Sync to blank |
| **Template improved** | ✅ Sync to blank |
| **Project name added** | ❌ Don't sync (make generic first) |
| **Project-specific example** | ⚠️ Sync if marked as example |
| **Bug fix in SOP** | ✅ Sync to blank |

- [ ] **Classify priority:**

| Priority | Definition | Example |
|----------|------------|---------|
| **Critical** | Affects core processes or contains bug fixes | Fix in ACTIVE_WORK_SOP that prevents conflicts |
| **High** | Significant improvement or new capability | New infrastructure pattern added |
| **Medium** | Enhancement or clarification | Improved wording in SOP |
| **Low** | Minor update or formatting | Fixed typo |

**Output:** Prioritized list of changes to sync

---

### Step 3: Create Audit Report

**Document findings:**

- [ ] **Create audit report:**
  ```bash
  cd ~/Github/blank
  cat > INFRASTRUCTURE_AUDIT_YYYY-MM-DD.md << 'EOF'
  # Infrastructure Template Audit
  
  **Date:** YYYY-MM-DD
  **Auditor:** [Agent/Person]
  **Projects Checked:** SourceApp
  **Blank Template:** ~/Github/blank/
  
  ---
  
  ## Summary
  
  - Files checked: [count]
  - Differences found: [count]
  - Critical updates needed: [count]
  - High priority updates: [count]
  - Medium/Low updates: [count]
  
  ---
  
  ## Findings
  
  ### Critical (Sync Immediately)
  
  | File | Project | Change | Priority |
  |------|---------|--------|----------|
  | [path] | [project] | [description] | Critical |
  
  ### High Priority (Sync Soon)
  
  | File | Project | Change | Priority |
  |------|---------|--------|----------|
  
  ### Medium/Low (Sync When Convenient)
  
  | File | Project | Change | Priority |
  |------|---------|--------|----------|
  
  ---
  
  ## Already Synced
  
  Files that are identical between projects and blank.
  
  ---
  
  ## Recommendations
  
  1. Sync critical updates immediately
  2. Schedule high priority updates this week
  3. Batch medium/low updates for next maintenance window
  
  ---
  
  **Next audit:** [Date + 1 month]
  EOF
  ```

**Output:** Audit report created

---

### Step 4: Sync Critical and High Priority Changes

**For each critical or high priority change:**

- [ ] **Follow Infrastructure Template Sync SOP:**
  - See: `INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md`
  - Copy file from project to blank
  - Update inventory if new file
  - Document sync

- [ ] **Verify sync:**
  ```bash
  diff [project-file] ~/Github/blank/[same-path]
  # Should show no differences
  ```

- [ ] **Mark as synced** in audit report

**Output:** Critical/high changes synced to blank

---

### Step 5: Verify Blank Has All Inventory Files and Check for New Infrastructure

**5a. Ensure every file in CORE_INFRASTRUCTURE_INVENTORY exists in blank:**

- [ ] **Open** `docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md` and list every reusable file path.
- [ ] **For each path,** check that `~/Github/blank/[path]` exists. If missing, copy from project to blank, then log in `~/Github/blank/INFRASTRUCTURE_VERSION_LOG.md`.
- [ ] **Result:** Blank directory contains all files listed in the inventory.

**5b. Find new project-agnostic files not yet in inventory or blank:**

- [ ] **Scan Layer 7:** `ls docs/07-guides/INFRASTRUCTURE_*.md` — compare to blank; any in project but not in blank or inventory must be added to inventory and synced to blank.
- [ ] **Scan Layer 6:** `find docs/06-operations -name "*_SOP.md" -o -name "*_PROCESS.md"` — compare to CORE_INFRASTRUCTURE_INVENTORY and to blank; any project-agnostic SOP/process not in inventory must be added to inventory and synced to blank.
- [ ] **Scan Layer 1:** Orientation files (except PROJECT_IDENTITY contents) — ensure all are in inventory and in blank.

- [ ] **For each new infrastructure file found:** Add entry to CORE_INFRASTRUCTURE_INVENTORY, copy file to `~/Github/blank/[same-path]`, log in INFRASTRUCTURE_VERSION_LOG, and ensure linked in docs/README.md.

**Output:** Blank is complete (all inventory files present); any new infrastructure has been added to inventory and synced to blank.

---

### Step 6: Update Blank Template Metadata

**Update template documentation:**

- [ ] **Update MANIFEST.md:**
  ```bash
  cd ~/Github/blank
  edit MANIFEST.md
  ```
  - Update file count
  - Update version if significant changes
  - Add version history entry

- [ ] **Update INFRASTRUCTURE_VERSION_LOG.md:**
  ```bash
  cat >> INFRASTRUCTURE_VERSION_LOG.md << 'EOF'
  ## YYYY-MM-DD — Audit and Sync
  
  **Audited by:** [Agent/Person]
  **Changes synced:** [count] files
  **Critical:** [list]
  **High:** [list]
  **New infrastructure added:** [list]
  
  ---
  EOF
  ```

- [ ] **Update CORE_INFRASTRUCTURE_INVENTORY.md** (if structure changed)

**Output:** Template metadata current

---

## Audit Checklist

Use this checklist for each audit run:

```markdown
## Infrastructure Audit Checklist — YYYY-MM-DD

### Pre-Audit
- [ ] Blank template accessible: ~/Github/blank/
- [ ] Active projects accessible
- [ ] CORE_INFRASTRUCTURE_INVENTORY.md reviewed

### Audit Execution
- [ ] Verified blank has all files in CORE_INFRASTRUCTURE_INVENTORY (Step 5a)
- [ ] Checked for new infrastructure in project not yet in inventory or blank (Step 5b)
- [ ] Compared all Layer 1 files (orientation)
- [ ] Compared all Layer 6 files (operations)
- [ ] Compared all Layer 7 files (guides)
- [ ] Compared all Layer 8 structures (templates)
- [ ] Classified all differences by priority
- [ ] Created audit report

### Post-Audit Actions
- [ ] Synced critical changes (if any)
- [ ] Synced high priority changes (if any)
- [ ] Scheduled medium/low changes
- [ ] Updated MANIFEST.md
- [ ] Updated INFRASTRUCTURE_VERSION_LOG.md
- [ ] Updated CORE_INFRASTRUCTURE_INVENTORY.md (if new files)
- [ ] Committed to Git (if applicable)

### Next Steps
- [ ] Set calendar reminder for next audit (1 month)
- [ ] Share audit results with team
- [ ] Plan sync work for medium/low priority items
```

---

## Automation Script Template

**Optional: Automate comparison:**

```bash
#!/bin/bash
# infrastructure-audit.sh

echo "Infrastructure Template Audit - $(date +%Y-%m-%d)"
echo "================================================"

BLANK="$HOME/Github/blank"
SOURCEAPP="$HOME/Github/project_sourceapp/sourceapp"

# Check infrastructure files
INFRA_FILES=(
  "AI.md"
  "docs/README.md"
  "docs/01-orientation/GETTING_STARTED.md"
  "docs/01-orientation/BOOTSTRAP_NEW_PROJECT.md"
  "docs/01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md"
  # Add all infrastructure files from inventory
)

echo ""
echo "Checking SourceApp vs Blank:"
for file in "${INFRA_FILES[@]}"; do
  if ! diff -q "$SOURCEAPP/$file" "$BLANK/$file" > /dev/null 2>&1; then
    echo "  ⚠️  DIFF: $file"
  fi
done

echo ""
echo "Audit complete. Review differences above."
```

---

## Success Metrics

| Metric | Target | Measure |
|--------|--------|---------|
| **Audit frequency** | Monthly | Last audit date |
| **Sync lag** | < 1 week for critical | Days between change and sync |
| **Template completeness** | 100% of infrastructure | Files in blank / total infrastructure files |
| **Drift detection** | All differences found | Audit report completeness |

---

## Common Issues

### Issue 1: Project Has Newer Infrastructure

**Problem:** Project has improved SOP, blank is outdated

**Solution:** Sync project → blank (follow sync SOP)

---

### Issue 2: Blank Has Updates, Projects Are Behind

**Problem:** Blank was updated but not synced to active projects

**Solution:** Sync blank → projects (reverse direction)

---

### Issue 3: Infrastructure Has Project-Specific Content

**Problem:** File has hardcoded project names or domain-specific examples

**Solution:**
1. Make generic (replace with "this project" or "see PROJECT_IDENTITY")
2. Mark examples as examples
3. Then sync to blank

---

## Related Documentation

- [Infrastructure Template Sync SOP](INFRASTRUCTURE_TEMPLATE_SYNC_SOP.md) — How to sync changes
- [Core Infrastructure Inventory](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md) — What is infrastructure
- [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md) — How new projects use blank

---

## Notes

- Run this audit monthly or after major infrastructure work
- Critical changes should be synced immediately (don't wait for next audit)
- Document all audits for traceability
- Blank template should be version controlled (Git)
- Consider automating comparison with script

---

**Last Updated:** 2026-01-28  
**Next Audit Due:** 2026-02-28 (monthly)
