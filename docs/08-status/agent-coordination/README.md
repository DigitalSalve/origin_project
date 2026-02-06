# Agent Coordination

**Coordination lives in GitHub Project.** This folder contains **informative docs** — read them first, then go to Project to take action.

---

## Read first (informative)

| File | Purpose |
|------|---------|
| [AGENT_COORDINATION_QUICK_START](AGENT_COORDINATION_QUICK_START.md) | Session start flow: read docs → go to Project |
| [AGENT_DIRECTIVES](AGENT_DIRECTIVES.md) | What directives are, where they live (Project → Directives view) |
| [AGENT_COORDINATION_LOG](AGENT_COORDINATION_LOG.md) | How to ask questions (create `coordination:q-a` items) |
| [AGENT_PROMPTS](AGENT_PROMPTS.md) | Role definitions, Cursor rule text |
| [SUBAGENT_TRANSITION_PLAN](SUBAGENT_TRANSITION_PLAN.md) | Architect + Cursor subagents; transition from separate chats |

**Flow:** Read these → Open Project → Follow directives, create Q&A items as needed.

---

## Setup

- **Setup guide:** [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md) — Run when creating a new project
- **Session start:** Read [AGENT_COORDINATION_QUICK_START](AGENT_COORDINATION_QUICK_START.md), then Project → Directives view
- **Q&A:** Project items with label `coordination:q-a`; add role label for target scope

---

## Why GitHub

- **Worktree consistency** — Coordination in `docs/08-status/` diverges across worktrees. GitHub Project is external; same view everywhere.
- **Single source of truth** — Directives and Q&A live in one place.
- **No sync** — No manual sync between main repo and worktrees.

---

## This folder

This folder contains **informative docs** (static). It may also contain:
- Breakout docs (expanded exchanges) — link from Project items

**Do not** write live directives or Q&A here. Use GitHub Project.

---

## Related

- [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)
- [Agent Session Hook SOP](../../06-operations/coordination/AGENT_SESSION_HOOK_SOP.md)
- [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)
