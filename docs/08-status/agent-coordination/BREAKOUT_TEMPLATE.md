# Agent Coordination Breakout — Template

**Purpose:** Template for breakout documents when an agent-coordination exchange expands into a dedicated doc. Copy this when creating `08-status/agent-coordination/YYYY-MM-DD_TOPIC.md` or similar.

**Source:** Link from the originating [AGENT_COORDINATION_LOG](AGENT_COORDINATION_LOG.md) entry with `**Breakout:** [This doc](path).`

---

## Template (copy below)

```markdown
# YYYY-MM-DD — [Topic Title]

**Purpose:** Breakout from [AGENT_COORDINATION_LOG](AGENT_COORDINATION_LOG.md) entry on YYYY-MM-DD. [One-line summary.]

**Participants:** [From Agent] ↔ [To Agent]

---

## Summary

[2–3 sentences: what was discussed, what was decided.]

---

## Exchange (condensed or full)

[Optional: Key Q&A or full thread. Or "See log entry for full exchange."]

---

## Outcomes

- **Decisions:** [What was decided.]
- **Action items:** [Who does what.]
- **Docs updated:** [Links to canon, architecture, behavior, or other docs that were updated.]

---

## Related

- [Log entry](AGENT_COORDINATION_LOG.md) — Original exchange
- [Relevant doc](path) — If outcome was written elsewhere
```

---

## When to use

- Exchange grows beyond a few back-and-forths
- Outcome needs its own doc for reference or follow-up
- Alignment meeting or state review produces decisions

## When to break out elsewhere

If the outcome belongs in another layer, create/update that doc instead (or in addition):

- **Canon** → `docs/02-canon/`
- **Architecture** → `docs/03-architecture/` or Decision Log
- **Behavior** → `docs/04-behavior/`
- **Feature** → `08-status/feature-requests/`
- **Product decision** → [Decision Log](../decisions/DECISION_LOG.md)

Always add `**Breakout:** [Doc](path)` to the log entry so the chain is traceable.
