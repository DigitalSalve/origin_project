# Agent Role Definitions

**Purpose:** Role labels, directive template, and coordination flow. **Implementation lives in Cursor subagents** (`.cursor/agents/`). See [SUBAGENT_TRANSITION_PLAN](SUBAGENT_TRANSITION_PLAN.md) for the Architect + subagents model.

---

## Hook command

When the user says "Report to the agent team" (or "Check in with agent coordination" / "Report to agent coordination"):

- **Cursor rule** `architect-coordination.mdc` applies — the agent behaves as **Architect**
- **Architect** runs session start, reads directives, responds to Q&A, and **delegates implementation to subagents**
- Subagent definitions: `~/.cursor/agents/` or project `.cursor/agents/` (architect, backend, web, webapp, mobile)

---

## Role labels (for Project items)

| Label | Subagent | Use when |
|-------|----------|----------|
| `agent:backend` | `/backend` | API, database, worker |
| `agent:web` | `/web` | Marketing site (React) |
| `agent:webapp` | `/webapp` | Web app / SPA |
| `agent:mobile` | `/mobile` | Flutter app |
| `agent:architect` | (main agent) | Directives, Q&A, infrastructure |

Each implementation subagent **documents its own work**; no separate Documentation agent.

---

## For the Architect

**When delegating:** Create the Project item first (directive or feature request with full instructions), then invoke the subagent with the item reference. See `~/.cursor/agents/architect.md` for the exact invocation format.

**Foreground vs background:**
- **Independent tasks** (e.g. backend + frontend changes that don't depend on each other) → invoke subagents in **parallel** (background).
- **Dependent tasks** (e.g. frontend needs new backend endpoint) → invoke in **sequence** (foreground): backend first, wait for result, then frontend with the endpoint details.

**When giving instructions:** Create a **Project item** with label `coordination:directive` and the appropriate role label(s).

**When you change infrastructure files:** Include in your directive: **Update worktrees** — commit any outstanding changes first, then `git fetch origin main && git merge origin/main`.

**When making requests:** Create a **Project item** with label `coordination:q-a`; add role label for target. See [AGENT_COORDINATION_LOG](AGENT_COORDINATION_LOG.md) for request/response format.

**Template for directive body:**

```markdown
## Context
(Why this directive exists.)

## Instructions
- (What agents need to do or provide.)
- (Number multiple items.)
- **If infrastructure changed:** Update worktrees — commit any outstanding changes first, then run `git fetch origin main && git merge origin/main` before continuing.

## Until
(Optional: when it expires or is superseded.)
```

---

## Related

- [AGENT_DIRECTIVES](AGENT_DIRECTIVES.md) — What directives are, where they live
- [AGENT_COORDINATION_LOG](AGENT_COORDINATION_LOG.md) — Request/response format
- [SUBAGENT_TRANSITION_PLAN](SUBAGENT_TRANSITION_PLAN.md) — Architect + subagents flow
- [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)
