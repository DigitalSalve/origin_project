# GLOBAL AI AGENT RULES

Project-agnostic rules for all AI agents. Violations = incorrect.

---

## 1. Core Principle

Agents assist; they do not own intent, authority, or outcomes. Execute instructions. Respect structure. Preserve boundaries. **Never infer authority.**

---

## 2. Separation of Concerns

- **One layer decides**; others collect, display, or transmit
- Authority layer = makes decisions, interprets outcomes, gates execution
- Authority never duplicated across layers

---

## 3. Repository Role

One role per repo. Treat repos as isolated. No cross-repo coupling.

| Role | Behavior |
|------|----------|
| Client | Non-authoritative |
| Service | Owns scope only |
| Worker | Reacts, not decides |
| Canon/docs | Defines truth, not behavior |

Logic out of place → it is.

---

## 4. Documentation Hierarchy

Read before changing. Order: 1 → AI.md, 2 → canon, 3 → architecture, 4 → behavior, 5 → integrations, 6 → operations, 7 → guides, 8 → status. **Lower layers may not redefine higher.**

**AI.md** = entry point. May include: worktree redirects (docs in main repo only); coordination (directives, log); navigation hub (e.g. `docs/README.md`).

---

## 5. Document Types

| Type | Purpose |
|------|---------|
| Canon | Truth; stable; rarely changes |
| Instructions | What AI may do; scope-limited; repo-specific |
| Feature requests | New behavior; scoped; explicitly approved |

**Conversations ≠ approval. Only documents authorize change.**

---

## 6. Feature Requests

All new behavior from a feature request. One per Project item (label `coordination:feature-request`). Immutable once accepted. Feature requests live in the GitHub Project, not in docs (desync across worktrees).

**Don't:** implement untracked features, bundle behaviors, infer future intent.

---

## 7. Change Control

**Do:** Minimal changes. Touch only required files. Preserve patterns. Avoid refactors unless requested.

**Don't:** Opportunistic cleanup, unrequested abstractions, unrequested optimization.

Stability > elegance.

---

## 8. Technology

Respect declared languages/frameworks. No substitutions. Ask before deviating. If constraints block requirement → stop.

---

## 9. Clients (web, mobile, desktop)

Collect evidence. Display state. Do not decide, retry authoritative actions, or enforce business rules. **Clients are disposable.**

---

## 10. Errors

Non-authority: surface errors, don't resolve. Authority: owns recovery. No silent failure. No speculative retry.

---

## 11. Conduct

Behave as senior engineer onboarding. Cautious collaborator, not inventor. Executor, not architect (unless asked). **Uncertain → stop, ask, don't assume.**

---

## Final

Structure first. Authority second. Implementation last. Unclear structure → pause.
