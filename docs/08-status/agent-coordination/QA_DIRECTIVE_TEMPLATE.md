# Q&A Directive Template

**Use when:** A subagent needs Architect's expertise, decision, or clarification on a topic.

**Label:** `coordination:q-a` + role label (e.g. `agent:backend`)

---

## Title Format

```
Q&A: [Agent Name] - [Topic]
```

Examples:
- Q&A: Backend Agent - API versioning strategy
- Q&A: Mobile Agent - Navigation architecture decision
- Q&A: Web Agent - SEO meta tag standards

---

## Body Template

```markdown
## Context

[Brief background on why this question arose]

## Questions

1. **[Question 1]**
   - Current state: [what exists]
   - Options: [A, B, C]
   - Preference: [if any]

2. **[Question 2]**
   [...]

## Investigation

[What you've researched/found so far]

## Blocking

[If blocking work] This blocks: [list items]
[If not blocking] Not blocking current work.

---

**Note to Architect:** When you respond with decisions, re-invoke me to acknowledge and close this issue.
```

---

## Architect Workflow

### 1. Subagent Posts Questions

Subagent investigates, posts questions, reports "Questions posted, awaiting Architect response."

### 2. Architect Responds

Read questions, make decisions, post response:

```markdown
## Architect Response

### Decision 1: [Topic]

**Decision:** [Choice]

**Rationale:** [Why]

**Action:** [What subagent should do]

### Decision 2: [Topic]

[...]

---

Status: Decisions provided. Re-invoking [agent] to acknowledge and close.
```

### 3. Architect Re-invokes Subagent

```python
Task(
  subagent_type="backend",  # or web, mobile, etc.
  prompt="""Your task: Acknowledge Architect response on Project item #[N]

URL: https://github.com/[owner]/[repo]/issues/[N]
MAIN_REPO_PATH: [path]
Workspace: [path]

The Architect has provided decisions on your questions. Read the response, 
post final acknowledgment report, set Status to Resolved, and close.

Report format:
## Report

Acknowledged Architect decisions:
- [List key decisions]

Actions taken: [if any]

Status: Resolved per Architect response.
"""
)
```

### 4. Subagent Closes

Subagent posts final report, sets Status to Resolved, reports complete.

---

## Why This Pattern

**Problem:** Subagents are temporary and lose context between invocations. If not re-invoked, Q&A items either:
- Stay open indefinitely
- Get closed by Architect (breaks subagent ownership)

**Solution:** Two-phase pattern ensures subagent owns the full lifecycle (question → acknowledgment → closure).

---

## Example

See issues #1-4 for **anti-pattern** (Architect closed directly, subagents never acknowledged).

For future Q&A directives, follow this template for proper two-phase closure.

---

**Created:** 2026-02-06  
**Related:** [AGENT_COORDINATION_SOP](AGENT_COORDINATION_SOP.md), [Architect agent definition](../../../.cursor/agents/architect.md)
