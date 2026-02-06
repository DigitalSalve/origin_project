# Agent Coordination Log — Informative

**Purpose:** Explains how to make requests and respond to other agents. **Requests and responses live in GitHub Project.** Use Project items with label `coordination:q-a`.

**Process:** [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)

---

## Where requests and responses live

- **Request:** Create a **Project item** with label `coordination:q-a` when you need something from another agent (question, implementation, clarification).
- **Response:** The target agent adds a **comment** to the item with Answer, Changes enacted, and Report. Do **not** append to a doc in this folder. Project is the channel.

---

## When you have a request for another agent

**Option:** Create Project item (below) or [invoke sub-agent](#when-you-have-an-implementation-question) if the request is blocking.

1. **Create** a Project item with label `coordination:q-a`.
2. **Add** role label for target scope (e.g. `agent:architect` for questions to Architect).
3. **Set** Project field to project slug (from PROJECT_IDENTITY).
4. **Assign** to user. The target agent (by label) will see it when they run.
5. **Use** this format in the item body:

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

---

## When you are the target agent

**Expected actions:**

1. **Read** Project items with label `coordination:q-a` where you are the target (by role or assignee).
2. **Respond** by adding a response (comment). Use this format:

```markdown
## Answer
[Your answers. Number if multiple questions.]

## Changes enacted
[What you implemented. List files/endpoints. "None" if questions only.]

## Report
[Summary: answered, implemented, sub-agents invoked, docs to update.]
```

3. **Enact** any requested changes. If work spans another domain, invoke that agent as sub-agent.
4. **Update** Status to Resolved when done.

---

## When you have an implementation question

If a question comes up during implementation (e.g. "Does the backend support X?"):

| Option | When |
|--------|------|
| **Create Project item** | Question can wait; target the relevant agent (e.g. `agent:backend`). |
| **Invoke sub-agent** | Question is blocking; invoke the other agent as sub-agent to get the answer in context. |

---

## Paths

**AI.md and docs are main-repo only.** **MAIN_REPO_PATH** is at the top of AI.md. Read this file at `[MAIN_REPO_PATH]/docs/08-status/agent-coordination/`.

---

## Related

- [AGENT_DIRECTIVES](AGENT_DIRECTIVES.md) — What directives are
- [AGENT_PROMPTS](AGENT_PROMPTS.md) — Role definitions
- [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)
