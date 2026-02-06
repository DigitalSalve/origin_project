# Agent Session Hook — SOP

**Purpose:** Defines the **hook command** — when the user says "Report to the agent team" (or equivalent), the agent connects to the coordination flow as the **Architect**.

**Status:** Active (GitHub-first)  
**Applies To:** Architect (single coordination agent)  
**Prerequisite:** Project has run [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)

---

## The hook command

| Command | Use when |
|---------|----------|
| **Report to the agent team** | Default |
| **Check in with agent coordination** | Shorter |
| **Report to agent coordination** | Alternative |

When the user gives one of these, the **Cursor rule** `architect-coordination.mdc` applies. The agent behaves as the Architect and follows the procedure in `~/.cursor/agents/architect.md`.

---

## Procedure (what the Architect does)

0. **Worktree hygiene** — Run `git worktree list`; for each worktree, run `git status`. Commit any uncommitted changes.
1. **Derive** Project URL from repo (`https://github.com/{owner}/{repo}/projects`). If repo unclear, ask the user.
2. **Verify** the project is for this workspace's repo. Do not use `gh project list` without a repo filter.
3. **Read** Directives view. Filter by **Project** field = project slug when applicable.
   - Follow any instructions that target Architect or "All agents."
   - Treat directives as assignments; they take precedence until done or superseded.
4. **Check** for Project items with label `coordination:q-a` where Architect is target and **Status: Open**. Filter by **Project** field = project slug when applicable.
   - Respond to each: add comment with your answer; set Status to Resolved.
5. **Delegate implementation** — For implementation work: create Project item first, then invoke subagent (`/backend`, `/web`, `/webapp`, `/mobile`) with the item reference. Do not implement yourself.
6. **Then** proceed with the user's task (or with directive work if no other task was given).

---

## Rules

- **Do not skip** steps 1–4. The hook connects you to the coordination layer before other work.
- **Assignee:** Always the user. Use labels for role routing. Never assign to "agent."
- **If you have questions for another agent:** Create Project item with label `coordination:q-a`. Do not ask the user to relay.
- **If blocked:** Create your question as a Project item; do not proceed without clarity.

---

## Related

- [Agent Coordination SOP](AGENT_COORDINATION_SOP.md)
- [GitHub Coordination Setup Guide](../../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)
- [AGENT_COORDINATION_QUICK_START](../../08-status/agent-coordination/AGENT_COORDINATION_QUICK_START.md)
- [SUBAGENT_TRANSITION_PLAN](../../08-status/agent-coordination/SUBAGENT_TRANSITION_PLAN.md)

---

**Last Updated:** 2026-01-28
