# Agent Directives — Informative

**Purpose:** Explains what directives are and where they live. **Read this before going to Project.** Directives tell you what to do at session start.

**Where directives live:** GitHub Project → Directives view. Label: `coordination:directive`.

**Process:** [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)

---

## What are directives?

The **Architect Agent** (or coordination management) creates Project items with label `coordination:directive` to tell agents what they need — priorities, alignment, context, instructions.

---

## At session start (Architect)

You are the **Architect**. You read **all** directives and **manage** role tasks by delegating.

1. **Read** [AGENT_COORDINATION_QUICK_START](AGENT_COORDINATION_QUICK_START.md) first.
2. **Open** GitHub Project (derive URL from repo: `https://github.com/{owner}/{repo}/projects`).
3. **Go to** Directives view. Filter by label `coordination:directive`.
4. **Read** all directive items (agent:architect, agent:backend, agent:web, agent:webapp, agent:mobile, "All agents").
5. **For each directive targeting a role** (agent:backend, agent:web, etc.): **delegate** — create a Project item with the task (or use the directive as the item), then invoke the subagent (`/backend`, `/web`, `/webapp`, `/mobile`) with that item. **You do not implement role tasks yourself.**
6. **For directives targeting Architect or "All agents"**: respond directly (answer, create follow-up items, etc.). When done, update Status or close as appropriate.

---

## Directive format (what you'll see in Project)

Directives in Project typically have:

- **Context** — Why this directive exists
- **Instructions** — What you need to do or provide
- **Until** — When it expires or is superseded

---

## Paths

**AI.md and docs are main-repo only.** **MAIN_REPO_PATH** is at the top of AI.md. Read these files at `[MAIN_REPO_PATH]/docs/08-status/agent-coordination/`. Do not read from your worktree.

---

## Related

- [AGENT_PROMPTS](AGENT_PROMPTS.md) — Role definitions
- [AGENT_COORDINATION_LOG](AGENT_COORDINATION_LOG.md) — How to ask questions (Q&A format)
- [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)
