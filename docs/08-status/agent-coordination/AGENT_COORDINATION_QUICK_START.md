# Agent Coordination — Quick Start

**Purpose:** Session-start flow for the **Architect**. Read this when the user says "Report to the agent team" (or similar). Then go to Project.

**Implementation:** The Cursor rule `architect-coordination.mdc` and `~/.cursor/agents/architect.md` define the full procedure. This doc summarizes the flow.

---

## Flow: You are the Architect

You **manage** all role tasks. You **delegate** implementation; you do not implement yourself.

0. **Worktree hygiene** — Run `git worktree list`; for each worktree, run `git status`. Commit any uncommitted changes before continuing.
1. **Read AI.md** at MAIN_REPO_PATH (workspace root).
2. **Read** [AGENT_DIRECTIVES](AGENT_DIRECTIVES.md) — What directives are, where they live.
3. **Open** GitHub Project. Derive URL from repo: `https://github.com/{owner}/{repo}/projects`. If repo unclear, ask the user.
4. **Directives view** — Read **all** directives. For each targeting a role (agent:backend, agent:web, agent:webapp, agent:mobile): **delegate** — create Project item with the task, invoke subagent with that item. For Architect or "All agents": respond directly.
5. **Q&A view** — Respond to items with label `coordination:q-a` where you are target. Add comment; set Status to Resolved.
6. **Delegate implementation** — All implementation work flows through you: create Project item first, then invoke subagent (`/backend`, `/web`, `/webapp`, `/mobile`) with the item reference. See [AGENT_PROMPTS](AGENT_PROMPTS.md) and `~/.cursor/agents/architect.md` for invocation format.

---

## Agent capabilities (use them)

Use `gh` CLI and env vars. Create Project items, run `gh` commands. GitHub, GitHub Project, Render, SendGrid are reachable. Do not defer to docs when you can act.

---

## Safeguards

- **Repo filter:** Only create or update Project items in the project for this workspace's repo. Derive repo from `git remote`; if unclear, ask.
- **Assignee:** Always the active user. Use labels for role routing.
- **Do not** create standalone reports in `docs/08-status/reports/` for coordination. Use Project.

---

## Paths — MAIN_REPO_PATH only

**AI.md and docs are read and updated only in the main repo.** **MAIN_REPO_PATH** is at the top of AI.md.

- **This folder:** `[MAIN_REPO_PATH]/docs/08-status/agent-coordination/`
- **Subagents:** `~/.cursor/agents/` or project `.cursor/agents/`

Do not read or update from your worktree.

**Full process:** [README](README.md) → [Agent Coordination SOP](../../06-operations/coordination/AGENT_COORDINATION_SOP.md)
