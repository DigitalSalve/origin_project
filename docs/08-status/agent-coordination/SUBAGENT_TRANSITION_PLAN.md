# Architect + Subagents Transition Plan

**Purpose:** Transition from separate agent chats to a single Architect agent that invokes Cursor subagents.  
**Status:** Discussion / Planning  
**Date:** 2026-01-28  
**Reference:** [Cursor Subagents docs](https://cursor.com/docs/context/subagents)

---

## Current Setup vs Proposed

| Aspect | Current | Proposed |
|--------|---------|----------|
| **Chats** | Separate chat per role (Backend, Web, Webapp, Mobile, Architect, Documentation) | Single chat: Architect only |
| **Invocation** | User opens specific agent chat; coordination via GitHub Project | User talks to Architect; Architect invokes subagents (`/backend`, `/web`, etc.) |
| **Coordination** | Project items with labels route to agents; each agent reads directives in its session | Architect reads directives, delegates to subagents; subagents receive task + context from Architect |
| **Context** | Each chat has its own history; agents read AI.md, Quick Start at session start | Subagents start with **clean context** — Architect creates Project item first, then invokes subagent with "look at Project item #N for instructions" |

---

## How Cursor Subagents Work

From the [Cursor docs](https://cursor.com/docs/context/subagents):

1. **Single-level only** — Subagents cannot invoke other subagents. Each implementation subagent documents its own work (no separate Documentation subagent).

2. **Clean context** — Each subagent starts fresh. Parent must include all relevant context in the prompt (project path, AI.md location, task, Project item refs).

3. **Foreground vs background** — Foreground blocks until done; background returns immediately. Use foreground for sequential work, background for parallel.

4. **User vs project subagents** — User subagents (`~/.cursor/agents/`) work across all projects. Project subagents (`.cursor/agents/`) are repo-specific. We want **user-level** for sourceapp, origin_project, future projects.

5. **Invocation** — `/backend implement the auth endpoint` or "Use the backend subagent to implement the auth endpoint." Parallel: "Review the API changes and update the documentation in parallel."

---

## Proposed Architecture

```
User → Architect (main chat)
         │
         ├─ /backend    → Backend subagent (API, DB, worker)
         ├─ /web        → Web subagent (marketing site, React)
         ├─ /webapp     → Webapp subagent (web app, SPA)
         ├─ /mobile     → Mobile subagent (Flutter)
         └─ /documentation → Documentation subagent (docs from responses)
```

**Architect responsibilities:**
- Session hook: worktree hygiene, read directives, respond to Q&A
- Own Project items; create directives; respond to coordination requests
- Delegate implementation to subagents (parallel when possible)
- When Backend reports "docs need updating" → Architect invokes Documentation with the context
- Report combined outcomes to user

**Subagent responsibilities (unchanged from current mandates):**
- Read AI.md at path provided by Architect
- Follow coordination rules (Project, docs hierarchy)
- Implement the requested task
- Report back: what was done, what needs documenting

---

## What Architect Must Pass to Subagents

Because subagents start with clean context, Architect's invocation prompt must include:

1. **Project item** — "Look at Project item #N (or URL) for your instructions." The item body contains the full task.
2. **MAIN_REPO_PATH** — e.g. `~/Github/project_sourceapp/sourceapp` (or origin_project, or future project)
3. **Workspace path** — Current worktree if different
4. **Repo** — For `gh` commands (derive from `git remote`)

---

## User Subagents

Location: `~/.cursor/agents/` (works across sourceapp, origin_project, future projects)

| File | Name | Description (when Architect invokes) |
|------|------|-------------------------------------|
| `architect.md` | architect | Coordinates work; creates Project items; invokes subagents with "look at Project item #N" |
| `backend.md` | backend | Backend, API, database, worker. Documents own work. Responds to Project item. |
| `web.md` | web | Web marketing site (React). Documents own work. Responds to Project item. |
| `webapp.md` | webapp | Web app / SPA. Documents own work. Responds to Project item. |
| `mobile.md` | mobile | Flutter mobile app. Documents own work. Responds to Project item. |

No separate Documentation subagent — each implementation agent documents its own work.

Each subagent:
- Reads the Project item the Architect points to for instructions
- Reads AI.md at MAIN_REPO_PATH
- Implements, documents own work, responds to Project item, sets Status to Resolved

---

## Migration Steps

1. **User subagents** — `~/.cursor/agents/{architect,backend,web,webapp,mobile}.md` (no documentation subagent).
2. **Update Architect rule** — "You are the Architect. Create Project item first (directive or feature request). Invoke subagent with 'Look at Project item #N for your instructions.' Pass MAIN_REPO_PATH and workspace path."
3. **Retire separate agent chats** — Stop using Backend/Web/Webapp/Mobile as standalone chats. Use Architect only.
4. **Update coordination docs** — AGENT_PROMPTS, Quick Start: "Architect creates Project items, invokes subagents with item ref; subagents read item, implement, document own work, respond to item."
5. **Test** — Architect creates directive → invokes /backend with "look at Project item #X" → backend implements, documents, responds.

---

## Architect: Subagent vs Main Agent

Two ways to use Architect:

1. **Invoke as subagent** — User says `/architect report to agent coordination` or `/architect implement auth flow`. Architect subagent runs, does session hook, delegates to backend/mobile/etc.
2. **Main agent identity** — Configure the chat so the default agent *is* the Architect (Cursor rule: "You are the Architect. Your behavior: read ~/.cursor/agents/architect.md. Create Project items first, then invoke backend, web, webapp, mobile subagents with 'look at Project item #N for instructions.'"). User talks to Architect directly; no need to type `/architect`.

## Open Questions

1. **Architect Cursor rule** — Where does the Architect rule live? User-level rule for "Architect" chat, or project AI.md when Architect is the main agent?
2. **Project-specific MAIN_REPO_PATH** — Subagents are user-level; MAIN_REPO_PATH varies per project. Architect must pass it. Should we add a convention: "When in workspace X, MAIN_REPO_PATH = Y" in each project's AI.md?
3. **Parallel vs sequential** — When Architect has "implement auth in backend and mobile," invoke both in parallel. When "backend then docs," invoke sequentially.
4. **Verifier subagent** — Cursor docs suggest a verifier subagent for "validate completed work." Add one?

---

## Related

- [AGENT_PROMPTS](AGENT_PROMPTS.md) — Current role definitions
- [AGENT_COORDINATION_QUICK_START](AGENT_COORDINATION_QUICK_START.md) — Session flow
- [Cursor Subagents](https://cursor.com/docs/context/subagents)
