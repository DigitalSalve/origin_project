# Agent Coordination — SOP

**Purpose:** Standard procedure for **agent coordination** — (1) Architect tells agents what they need, (2) agents ask and respond to each other. **Coordination lives in GitHub Project** for worktree consistency.

**Status:** Active (GitHub-first)  
**Applies To:** All agents (Backend, Web, Webapp, Mobile App, Architect, Documentation, etc.)  
**Prerequisite:** Project has run [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)

---

## Overview: Two flows

| Flow | Direction | Where | Purpose |
|------|-----------|-------|---------|
| **Architect directives** | Architect → Agents | GitHub Project → Directives view | Architect tells agents what they need |
| **Agent requests and responses** | Agent ↔ Agent | GitHub Project (label `coordination:q-a`; add role label for target scope) | Agents create requests; target agent adds response (comment) by label |

**All agents:** Read Directives view at session start. Check items with labels matching your role (e.g. `agent:backend`).

**Assignee:** Always the GitHub user (active user by default). Agents never assign to "agent." Use labels for role routing.

**Project URL:** Derive from repo: `https://github.com/{owner}/{repo}/projects`. Ask user if repo unclear.

**Safeguard:** Only create or update items in the project for this workspace's repo. Before creating or updating items, confirm the Project URL matches your workspace repo (from `git remote`). When reading items, filter by **Project** field = project slug from PROJECT_IDENTITY to ensure you only act on items for this repo. Do not use `gh project list` without a repo filter.

---

## 1. When to use

- **You have a request for another agent** — Create Project item with label `coordination:q-a`; add role label (e.g. `agent:architect`); assign to user.
- **You need alignment on project state** — Create Project item or post to Discussion.
- **Administrative coordination** — Handoffs, clarifications, decisions that span agent boundaries.
- **Not for:** Feature implementation (use feature requests), file check-out (use Active Work SOP), gap tracking (use IMPLEMENTATION_VERIFICATION), or user-facing decisions (use Decision Log).

---

## 2. Where to write

| Output | Location | Who writes |
|--------|----------|------------|
| **Architect directives** | GitHub Project → Directives view (new item) | Architect Agent |
| **Requests and responses** | GitHub Project item (label `coordination:q-a`; role label for target); request in body, response in comment | Any agent |
| **Larger coordination reports** | `08-status/agent-coordination/YYYY-MM-DD_TOPIC.md` — link from Project item | Architect or Documentation Agent (when invoked) |

---

## 3. Format (requests and responses)

### Creating a request

**Title:** `YYYY-MM-DD | From Agent → To Agent | Subject`

**Body:**
```markdown
## From
[Your role] → [Target agent]

## Subject
[One-line subject]

## Request
[Your questions or requests. Number if multiple.]

## Status
Open
```

**Labels:** `coordination:q-a`, plus role label (e.g. `agent:architect`)  
**Project field:** Set to project slug (e.g. `sourceapp`)  
**Assign:** User (active user by default; never assign to "agent")

### Response format (target agent's comment)

When you are the target agent, add a comment in this format:

```markdown
## Answer
[Your answers to each question. Number if multiple.]

## Changes enacted
[What you implemented, if any. List files/endpoints changed. "None" if questions only.]

## Report
[Summary of what was done. Include: answered questions, implemented changes, sub-agents invoked (if any), docs to update (if any).]
```

**Rules:**
- Use consistent agent names: Backend Agent, Web Agent, Architect Agent, etc.
- One logical topic per item. When responding: add comment in format above; set Status to Resolved.

---

## 4. Process steps

### At session start (all agents)

1. **Read** [AGENT_COORDINATION_QUICK_START](../../08-status/agent-coordination/AGENT_COORDINATION_QUICK_START.md) and [AGENT_DIRECTIVES](../../08-status/agent-coordination/AGENT_DIRECTIVES.md) — informative docs that explain the flow.
2. **Derive** Project URL from repo (`/projects`); ask user if unclear.
3. **Verify** the project is for this workspace's repo before proceeding.
4. **Open** Directives view — follow instructions for your role or "All agents." Filter by **Project** field = project slug when multiple repos share a project.
5. **Check** for Project items with label `coordination:q-a` where a role label matches your scope (e.g. `agent:backend`). Filter by **Project** field = project slug when applicable.
6. **Respond** to each request: add response (comment) per [Response format](#response-format-target-agent-adds-comment); enact any requested changes; report what was done; set Status to Resolved. See [When you are the target agent](#when-you-are-the-target-agent-to-agent) for full steps.

### Architect: When you need to tell agents something

1. **Create** new item in Directives view.
2. **Title:** Date, To (which agents), Context.
3. **Body:** Instructions. Add role labels (e.g. `agent:backend`). Set **Project** field to project slug. Assign to user(s) as needed.
4. **Update** when directive is superseded (close item or add comment).

### When you have a request for another agent

1. **Create** Project item with label `coordination:q-a`.
2. **Title:** `YYYY-MM-DD | Your Role → Target Agent | Subject`
3. **Body:** Request, Status: Open.
4. **Set Project field** to project slug. **Assign** to user. Add role label for target scope (e.g. `agent:architect`).
5. **Do not** rely on the user to relay. The target agent will see the item by label.

### When you are the target agent (To Agent)

**Expected actions (in order):**

1. **Check** Project for items with role label matching your scope.
2. **Answer** in a comment (use [Response format](#response-format-target-agent-adds-comment) above).
3. **Enact** any requested changes (implement in code/config). If work spans another domain (e.g. webapp + backend), invoke those agents as **sub-agents** (in parallel when independent); you remain primary, answer/coordinate, and report the combined outcome. See [AGENT_PROMPTS](../../08-status/agent-coordination/AGENT_PROMPTS.md) Sub-agents and parallel delegation.
4. **Report** in your comment: what was answered, what was implemented, sub-agents invoked (if any), docs to update (if any).
5. **Update** Status to Resolved.
6. **Do not update docs yourself.** Invoke **Documentation Agent** as sub-agent or note "Docs to update: [list]" for standalone Documentation Agent. See [Documentation Agent SOP](DOCUMENTATION_AGENT_SOP.md).

### When to breakout into another document

Break out when the exchange grows or the outcome belongs in canon, architecture, behavior, or another layer. **Documentation Agent** creates the doc (invoke as sub-agent or note in your comment for standalone). Add comment to Project item: `**Breakout:** [Doc name](path)`. Promote to versioned docs per [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md#projects--repo-docs-flow).

---

## 5. Related

- [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) — Run when creating new project
- [Agent Session Hook SOP](AGENT_SESSION_HOOK_SOP.md) — "Report to the agent team" triggers coordination check-in
- [Active Work SOP](ACTIVE_WORK_SOP.md) — Check-out/check-in via Project or docs
- [Decision Log](../../08-status/decisions/DECISION_LOG.md) — For product/behavior decisions

---

**Last Updated:** 2026-01-28
