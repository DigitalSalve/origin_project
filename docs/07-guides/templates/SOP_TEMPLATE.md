# SOP Template

**Purpose:** Standardized template for creating Standard Operating Procedures (SOPs). Use this when authoring any new SOP so that structure, tone, and required sections are consistent across the doc base.

**How to use:** Copy this template into a new file (e.g. `docs/06-operations/{SOP_NAME}_SOP.md`). Replace every `[placeholder]` and _italic_ prompt with the specific content for your SOP. Remove optional sections that do not apply. When the SOP produces a deliverable (e.g. a report), fill in the **Output / Deliverable** and **Report Template** sections; otherwise omit or shorten them.

---

# [SOP Title]

**Purpose:** [One to three sentences describing what this SOP is for and when to use it.]

**Status:** [Active | Draft | Deprecated]

**Applies To:** [Who or what the SOP applies to — e.g. "All repositories", "Backend services", "Migration work", "AI agents". Be specific enough that a reader knows whether the SOP applies to their context.]

---

## Overview

This SOP defines a systematic process to:

1. **[First goal]** — [Short description]
2. **[Second goal]** — [Short description]
3. **[Third goal]** — [Short description]
4. **[Fourth goal, if any]** — [Short description]

_[If the SOP produces a named deliverable (e.g. a report), add one sentence:] The output is **[Deliverable Name]** that [serves what purpose — e.g. "serves as the authoritative snapshot of backend state for migration planning".]_

---

## Prerequisites

Before beginning _[the activity this SOP governs]_:

1. **Read relevant documentation:**
   - `AI.md` (repository-specific AI behavior rules)
   - `docs/README.md` (documentation navigation)
   - _[Add other doc paths that must be read — e.g. `docs/03-architecture/`, `docs/04-behavior/` — when present or always.]_

2. **Understand _[the domain/context]_:**
   - _[Bullet 1 — e.g. "Entry point: main server file and framework"]_
   - _[Bullet 2 — e.g. "Routing: how routes are mounted"]_
   - _[Bullet 3 — e.g. "Runtime and deployment"]_
   - _[Add or remove bullets as needed.]_

3. **_[Third prerequisite category — e.g. "Identify scope" or "Identify user roles"]_:**
   - _[Bullet 1]_
   - _[Bullet 2]_
   - _[Add or remove bullets as needed.]_

---

## Process

_[Use "Process", "Examination Process", "Workflow", or another term that fits the SOP. Keep it singular and consistent.]_

### Step 1: [Step Title]

_[One sentence describing the aim of this step.]_

#### [Sub-step or area, if needed — e.g. "1.1 Entry Point and Server"]

- [ ] **[Checklist item 1]:** [Description]
- [ ] **[Checklist item 2]:** [Description]
- [ ] **[Checklist item 3]:** [Description]
- _[Add or remove checklist items.]_

**What to look for:**
- _[Pattern, anti-pattern, or pitfall 1]_
- _[Pattern, anti-pattern, or pitfall 2]_
- _[Add or remove as needed.]_

**Output:** _[What this step must produce — e.g. "A Component Inventory table with columns: #, Component, Path, Type, Notes." If the step has no formal output, say "None" or "Mental model / notes only."]_

---

### Step 2: [Step Title]

_[Repeat the same pattern: aim of the step, sub-steps with checklists, "What to look for", "Output".]_

#### [Sub-step, if any]

- [ ] **[Checklist item]:** [Description]

**What to look for:**
- _[Item]_

**Output:** _[Description or "None".]_

---

### Step 3: [Step Title]

_[Repeat for each step. Most SOPs have 3–7 main steps.]_

---

### Step N: [Step Title]

_[Continue until the process is complete.]_

---

## Output / Deliverable

_[**Include this section only if the SOP produces a formal deliverable** (e.g. a report, a checklist, an artifact). Otherwise delete this section.]_

**Deliverable name:** _[e.g. "Screen Examination Report", "Backend Examination Report"]_

**Location and naming:** _[e.g. "docs/08-status/screen-examination/{YYYY-MM-DD}_SCREEN_EXAMINATION_REPORT.md"]_

**When to create:** _[e.g. "After completing Steps 1–N", "At the end of each examination run".]_

---

## Report Template

_[**Include this section only if the SOP produces a report or structured deliverable.** Provide a markdown skeleton that implementers must follow. Otherwise delete this section.]_

```markdown
# [Report Title]

**Date:** YYYY-MM-DD
**Examiner:** [AI Agent / named examiner]
**Repository:** [Repository name]
**Scope:** [Full application / bounded area]
**Process:** Per `docs/06-operations/[SOP_FILENAME].md`

---

## 1. Executive Summary

- _[Metric 1]:_ X
- _[Metric 2]:_ Y
- _[Add metrics relevant to this SOP.]_

**Key findings:** [2–4 bullet summary]

---

## 2. [Section Title]

_[Placeholder for main content sections. Use headings and tables as in existing Screen/Backend Examination reports.]_

---

## 3. [Section Title]

_[…]_

---

## N. Recommendations

1. _[Immediate — Critical]_
2. _[Short-term — High]_
3. _[Medium-term — Medium]_

---

## Next Steps

- [ ] _[Action 1]_
- [ ] _[Action 2]_
- _[Add or remove.]_

---

**Last Updated:** YYYY-MM-DD
**SOP:** `docs/06-operations/[SOP_FILENAME].md`
```

---

## Classification / Severity

_[**Include this section only if the SOP uses fixed categories** (e.g. Working / Not Working / Needing Implementation, or Critical / High / Medium / Low). Define each level and when to use it. Otherwise delete this section.]_

| [Category name] | Definition | When to use |
|------------------|------------|-------------|
| **[Level 1]** | [One-sentence definition] | [Example or trigger] |
| **[Level 2]** | [One-sentence definition] | [Example or trigger] |
| **[Level 3]** | [One-sentence definition] | [Example or trigger] |
| _[Add or remove rows.]_ |

---

## Integration with Other Processes

_[**Include this section if the SOP touches other docs or processes.** List each related process and how this SOP interacts with it. Otherwise delete or shorten.]_

- **[Process or doc name]:** [How this SOP relates — e.g. "Screen report defines what the UI expects; this SOP defines what the API provides. Compare to derive gaps."]
- **[Another process]:** [Relationship.]
- _[Add or remove bullets.]_

---

## Notes

- _[Note 1 — e.g. "Do not fix issues during examination; document first, fix later."]_
- _[Note 2 — e.g. "When the backend is the migration source, treat it as read-only."]_
- _[Note 3 — e.g. "Focus on user-facing or migration-critical issues."]_
- _[Add or remove notes.]_

---

**Last Updated:** YYYY-MM-DD
