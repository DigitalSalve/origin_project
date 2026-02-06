# Project Documentation Hierarchy

**Purpose:** Single explanation of how this project’s documentation is organized. For humans and agents.

---

## What It Is

The **project documentation hierarchy** is an 8-layer structure that organizes all project documentation. Each layer has a distinct purpose. Documents live in numbered folders (`01-orientation` through `08-status`). The hub is `docs/README.md`, which links to every layer and process.

---

## Why It Exists

- **Shared source of truth** — Humans and AI agents use the same docs. No separate “agent docs” vs “human docs.”
- **Discoverability** — Start at the hub; follow links to the right layer. Nothing is orphaned.
- **Separation of concerns** — Canon (truth) is separate from architecture (construction), behavior (flows), operations (how-to), and status (progress).
- **Consistency** — Same structure across projects. When you move to a new project, the hierarchy is familiar; only the content changes.

---

## The 8 Layers

| Layer | Folder | Purpose |
|-------|--------|---------|
| 1 | `01-orientation` | How to approach the repo, project identity, getting started |
| 2 | `02-canon` | System truth, invariants, mandates (not suggestions) |
| 3 | `03-architecture` | Internal construction, data, boundaries |
| 4 | `04-behavior` | User flows, UI, API wiring |
| 5 | `05-integrations` | Third-party APIs, contracts |
| 6 | `06-operations` | SOPs, deployment, migration, documentation process |
| 7 | `07-guides` | Templates, post-completion reference |
| 8 | `08-status` | Progress, reports, feature requests, QA |

---

## How It Works

### Discovery flow

1. **AI.md** (repo root) — Entry point; coordination first (MAIN_REPO_PATH, static route); behavior rules. Then docs/README is the hub.
2. **docs/README.md** — Hub. Top section: "Agent: choose your path." Layer = structural path; Direct = shortcut. Use either; both lead to docs in the hierarchy.
3. **Layers** — Land in the layer; pick the doc from the layer index.

### Operations → reports

- **Layer 6** defines *how* to do things (SOPs, processes).
- **Layer 8** holds *outputs* (reports, status, feature requests).
- Each SOP that produces a report specifies where it goes. The chain stays: operations → reports → hub.

### For humans

- Start at [docs/README.md](../README.md).
- Use “When to Read This” and “Quick Reference by Role” to pick layers.
- When onboarding, read layers 1–4 in order.

### For agents

- Read AI.md. Go to docs/README. Choose your path — use Layer (structural) or Direct (faster). Both lead to docs in the hierarchy.
- Use “Quick Reference by Task” or “By Role” to load only relevant layers. "Agent: choose your path" table at top of docs/README.

---

## Key Documents

| Document | Role |
|----------|------|
| **AI.md** | Entry point; coordination first; behavior rules. Read and updated only at MAIN_REPO_PATH. Worktrees: code only; AI.md and docs stay in main repo. |
| **docs/README.md** | Hub; navigation; process table; layer index |
| **docs/02-canon/GUIDELINES.md** | Extended behavior rules (Code Quality, Security, etc.) |
| **docs/02-canon/PROJECT_SCOPE.md** | Product boundaries; in-scope vs out-of-scope |

---

## Principles

1. **Layers do not mix concerns** — Architecture docs don’t describe behavior; behavior docs don’t describe architecture.
2. **Canon is truth** — Layer 2 documents are mandates.
3. **Status is time-bound** — Layer 8 documents may become outdated; check dates.
4. **Read in order when it matters** — Layers 1–8 are ordered for a reason.
5. **When in doubt, ask** — If placement seems wrong, flag it.

---

## Where to Go Next

- **Navigation and links** → [docs/README.md](../README.md)
- **Setup and bootstrapping** → [GETTING_STARTED.md](GETTING_STARTED.md)
- **Project identity** → [PROJECT_IDENTITY.md](PROJECT_IDENTITY.md)
