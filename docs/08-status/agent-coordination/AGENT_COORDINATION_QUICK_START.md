# Agent Coordination — Quick Start

**Purpose:** Session-start flow for the **Architect**. Read this when the user says "Report to the agent team" (or similar). Then go to Project.

**Implementation:** The Cursor rule `architect-coordination.mdc` and `~/.cursor/agents/architect.md` define the full procedure. This doc summarizes the flow.

---

## Flow: You are the Architect

You **manage** all role tasks. You **delegate** implementation; you do not implement yourself.

0. **Worktree hygiene** — Run `git worktree list`; for each worktree, run `git status`. Commit any uncommitted changes before continuing.
1. **Read AI.md** at MAIN_REPO_PATH (workspace root).
2. **Read** [AGENT_DIRECTIVES](AGENT_DIRECTIVES.md) — What directives are, where they live.
3. **Read repo owner and Project URL** — From [PROJECT_IDENTITY](../../01-orientation/PROJECT_IDENTITY.md). Established during adoption; permanent.
4. **Owner check and access test:**
   - Get current user: `gh api user --jq .login`
   - If current user ≠ repo owner, notify: "Repo owner is [owner]; you are [current_user]. Project items live under [owner]'s account."
   - Test access: `gh project list --owner [owner]` or try to view a Project item
   - If access fails: "Cannot access [owner]'s projects. Contact [owner] to grant access to the Agent Coordination project."
5. **Open** GitHub Project. If Project does not exist, adoption was incomplete — tell user to run [INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP](../../06-operations/processes/INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md) Step 5 or [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md), then record repo owner and Project URL in PROJECT_IDENTITY.
6. **Directives view** — Read **all** directives. For each targeting a role (agent:backend, agent:web, agent:webapp, agent:mobile): **delegate** — create Project item with the task, invoke subagent with that item. For Architect or "All agents": respond directly.
7. **Q&A view** — Respond to items with label `coordination:q-a` where you are target. Add comment; set Status to Resolved.
8. **Delegate implementation** — All implementation work flows through you: create Project item first, then invoke subagent. Task tool if available; otherwise provide [manual subagent prompt](../../06-operations/coordination/AGENT_COORDINATION_SOP.md#manual-subagent-invocation-when-task-tool-unavailable). See [AGENT_PROMPTS](AGENT_PROMPTS.md) and `.cursor/agents/architect.md`.

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
