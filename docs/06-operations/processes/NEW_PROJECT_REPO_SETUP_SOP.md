# New Project Repo Setup — SOP

**Purpose:** Standard procedure for creating a new project repository and setting up the coordination infrastructure. Use when starting a project from the blank template.

**Status:** Active  
**Applies to:** New projects bootstrapped from blank template  
**Related:** [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md), [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)

---

## Overview

When starting a new project from the blank template, you must:

1. Create the GitHub repository
2. Populate it with the template
3. Set MAIN_REPO_PATH
4. Run GitHub coordination setup

This SOP defines the order and checks.

---

## Prerequisites

- Blank template (or template repo) available
- GitHub account with repo creation access
- `gh` CLI installed and authenticated (for coordination setup)

---

## Steps

### Step 1: Create the repository

**Option A — Use GitHub template (recommended):**

1. On GitHub: Create new repo from template
2. Select the blank template repo
3. Name the new repo (e.g. `my-project`)
4. Create repository
5. Clone: `git clone https://github.com/owner/my-project.git && cd my-project`

**Option B — Create empty repo, then copy:**

1. On GitHub: Create new repository (empty, no README)
2. Clone: `git clone https://github.com/owner/my-project.git`
3. Copy blank template contents into the clone (excluding `.git`)
4. `cd my-project`

### Step 2: Set MAIN_REPO_PATH

**MAIN_REPO_PATH** is the absolute path to the project root (where AI.md and docs/ live). Agents use it for worktree redirects and doc paths.

1. Determine the path: `pwd` in the project root (e.g. `/Users/you/Github/my-project`)
2. Edit `AI.md`: Replace the placeholder in the "If you are in a worktree" section:

   ```
   **MAIN_REPO_PATH:** /Users/you/Github/my-project
   ```

3. Optional: Add the same path to `docs/01-orientation/PROJECT_IDENTITY.md` under a "Main repo path" field if your project uses it.

**Rule:** For a single-checkout project (no worktrees), MAIN_REPO_PATH = workspace root. For worktrees, MAIN_REPO_PATH = the main repo path, not the worktree path.

### Step 3: Fill PROJECT_IDENTITY

Update `docs/01-orientation/PROJECT_IDENTITY.md` with project name, slug, repository role, and related repos. See [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md).

### Step 4: Run GitHub coordination setup

Follow [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md):

- Create Project "Agent Coordination"
- Link to repo
- Create labels
- Configure views
- Verify checklist

The blank template's AI.md already has the coordination section; ensure MAIN_REPO_PATH is set (Step 2).

### Step 5: Verify

- [ ] Repo exists on GitHub
- [ ] MAIN_REPO_PATH set in AI.md
- [ ] PROJECT_IDENTITY filled
- [ ] GitHub Project created and linked
- [ ] Labels created
- [ ] Views configured

---

## When MAIN_REPO_PATH changes

If you move the project (e.g. rename folder, change machine), update MAIN_REPO_PATH in AI.md. It must match the actual path to the project root.

---

## Related

- [Bootstrap New Project](../../01-orientation/BOOTSTRAP_NEW_PROJECT.md)
- [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)
- [Core Infrastructure Inventory](../../01-orientation/CORE_INFRASTRUCTURE_INVENTORY.md)

---

**Last updated:** 2026-01-28
