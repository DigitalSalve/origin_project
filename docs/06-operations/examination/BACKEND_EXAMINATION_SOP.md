# Backend Examination SOP

**Purpose:** Standard Operating Procedure for systematically examining a backend codebase to identify key components and processes, and to produce a structured outline of the system **as-is** — including what is **working**, what is **not working**, and what **needs to be implemented**.

**Status:** Active

**Applies To:** Backend services (APIs, workers, databases, integrations). Use when the source backend is read-only (e.g. gop_app for migration) or when documenting any backend for migration or verification.

---

## Overview

This SOP defines a systematic process to:

1. **Inventory key components** — Entry point, routes, middleware, database layer, shared libs, and external integrations
2. **Identify key processes** — Auth, CRUD flows, background jobs, notifications, and domain-specific workflows
3. **Examine each component and process** — Existence, wiring, stubs, error handling, and test coverage
4. **Produce a structured outline** — A single report that classifies the system **as-is** into: **Working**, **Not Working**, and **Needing to be Implemented**

The output is a **Backend Examination Report** that serves as the authoritative snapshot of backend state for migration planning, gap analysis, and implementation verification.

---

## Prerequisites

Before beginning a backend examination:

1. **Read relevant documentation:**
   - `AI.md` (repository-specific AI behavior rules)
   - `docs/README.md` (documentation navigation)
   - `docs/03-architecture/` (system architecture, data models) when present
   - `docs/04-behavior/` (API matrices, flows) when present
   - `docs/05-integrations/` (external APIs, contracts) when present

2. **Understand the backend structure:**
   - **Entry point:** Main server/file (e.g. `hono.ts`, `index.js`, `main.go`)
   - **Routing:** How routes are mounted (Hono, Express, Fastify, etc.)
   - **Data layer:** DB client, ORM, migrations location
   - **Runtime:** Node/Bun/Deno, Go, etc.

3. **Identify scope:**
   - Full backend, or a bounded area (e.g. “auth and users only”)
   - Whether the backend is **read-only** (e.g. source for migration) or under active development

---

## Examination Process

### Step 1: Inventory Key Components

Create a structured list of backend building blocks. Do not yet classify; only identify and locate.

#### 1.1 Entry Point and Server

- [ ] **Entry file:** Path to the main server/bootstrap (e.g. `backend/hono.ts`, `src/index.ts`)
- [ ] **Framework and version:** e.g. Hono 4.x, Express 5.x
- [ ] **How the app is started:** Script, env, and any dev/prod differences
- [ ] **Mount points:** Base path(s) for API (e.g. `/api`, `/trpc`)

**Output:** One-row summary: entry path, framework, base path(s).

#### 1.2 Routes / Handlers

- [ ] **Route registry:** File or dir that registers routes (e.g. `hono.ts` mounting, or `routes/` convention)
- [ ] **List every route module:** For each file under `routes/` (or equivalent), record:
  - File path
  - Logical area (auth, users, news, events, etc.)
  - Exported HTTP methods/paths or tRPC procedures if applicable
- [ ] **Route-to-handler mapping:** For each route module, note the main handler(s) and any sub-routes

**Output:** A **Component Inventory** table:

| # | Component | Path | Type | Notes |
|---|-----------|------|------|-------|
| 1 | Server entry | backend/hono.ts | Entry | Hono app |
| 2 | Auth routes | backend/routes/auth.ts | Routes | Login, register, OAuth |
| 3 | … | … | … | … |

Use **Type** values such as: `Entry`, `Routes`, `Middleware`, `DB`, `Lib/Service`, `Integration`, `Worker`.

#### 1.3 Middleware

- [ ] **Middleware stack:** Order and location of global/route middleware (auth, CORS, rate-limit, logging, etc.)
- [ ] **Per-route or per-group middleware:** e.g. `admin`, `county`, `permissions`
- [ ] **File paths:** Where each middleware is defined and where it is applied

**Output:** Rows in the Component Inventory for each middleware, plus a short “stack order” description.

#### 1.4 Database Layer

- [ ] **DB client/ORM:** Path and type (e.g. Drizzle, Prisma, raw client)
- [ ] **Schema definition:** Single file or dir (e.g. `db/schema.ts`)
- [ ] **Migrations:** Dir (e.g. `db/migrations/`) and naming convention
- [ ] **Connection and pooling:** Where connection is created and how it is shared

**Output:** Component Inventory rows for schema, migrations, and connection; optional one-line “migration state” (e.g. “N migrations, last dated YYYY-MM-DD”).

#### 1.5 Shared Libraries / Services

- [ ] **Lib or services dir:** e.g. `backend/lib/`, `backend/services/`
- [ ] **List each module:** Auth helpers, permissions, rate-limit, geo lookups, notification dispatch, etc.
- [ ] **Dependencies:** Which routes or other libs use each module

**Output:** Component Inventory rows for each lib/service, with “Used by” (routes or components).

#### 1.6 External Integrations

- [ ] **Outbound APIs:** the state voter lookup, payment, email, SMS, push, etc.
- [ ] **Config and secrets:** Env vars or config files that gate integrations
- [ ] **Where they are called:** Route handlers, libs, or background jobs

**Output:** Component Inventory rows for each integration; note “config source” and “invoked from”.

#### 1.7 Background Jobs / Workers (if any)

- [ ] **Worker or job entry point:** Separate process or same process with a queue
- [ ] **Job types:** e.g. “send notification”, “sync external data”
- [ ] **Trigger:** Cron, queue, or on-demand from API

**Output:** Component Inventory rows and a short “job catalog”.

---

### Step 2: Identify Key Processes

Map **processes** (multi-step flows that span components). A process is something like “user login”, “create event and notify subscribers”, “submit donation and record payment”.

#### 2.1 Authentication and Authorization

- [ ] **Login / sign-in:** Path from “request with credentials” to “session/token”
- [ ] **Registration:** Path from “request with sign-up data” to “user record + session”
- [ ] **OAuth / third-party auth:** Which providers, where they’re wired, how tokens are exchanged
- [ ] **Session/token handling:** Where tokens are created, validated, and refreshed
- [ ] **Authorization:** How permissions/roles are checked (middleware, lib, per-route)

**Output:** Process list entry “Auth” with sub-flows: Login, Register, OAuth, Session, Authorization.

#### 2.2 Domain Processes

For each major domain (events, news, polls, deals, sweepstakes, donations, notifications, users, party/directory, etc.):

- [ ] **Create/update/delete flows:** Which routes and services are involved
- [ ] **Read/query flows:** List, get-by-id, filter, pagination
- [ ] **Side effects:** Notifications, emails, external API calls, state changes
- [ ] **Business rules:** Validation, limits, eligibility (e.g. deal redemption limits, sweepstakes eligibility)

**Output:** One process per domain (or per major use case), with bullet points for “create/read/update/delete”, “side effects”, “business rules”.

#### 2.3 Cross-Cutting Processes

- [ ] **Error handling:** Global handler, per-route handling, logging, and client-facing shape
- [ ] **Rate limiting:** Where applied, limits, and response when exceeded
- [ ] **Validation:** Request validation (schema, sanitization) and where it runs
- [ ] **Audit or logging:** What is logged, where, and whether it’s structured

**Output:** Short “Error handling”, “Rate limiting”, “Validation”, “Logging” process descriptions.

---

### Step 3: Examine Each Component and Process

For **each** component and process from Steps 1–2, apply the checks below. Record pass/fail and notes; do not yet assign “Working / Not Working / Needing Implementation” (that is Step 4).

#### 3.1 Component/File Existence and Wiring

- [ ] **File exists** at the path recorded in the inventory
- [ ] **File is not a stub:** Contains real logic (not only `throw new Error("not implemented")` or `return null`)
- [ ] **Imports resolve:** No broken module paths; build/lint passes for this area
- [ ] **Wired into the app:** Entry point or router actually imports and uses the component

**What to look for:** Missing files, empty/stub files, dead code (never imported), circular deps.

#### 3.2 Stubbed or Placeholder Behavior

- [ ] **No intentional stubs:** No `TODO`, `FIXME`, `STUB`, `PLACEHOLDER`, “not implemented”, “coming soon” in behavior-critical paths
- [ ] **No no-op implementations:** Handlers that return 200 but do nothing, or that always return empty/hardcoded data
- [ ] **No mock-only data in production paths:** If the backend is the source of truth, it should not serve only from in-memory mocks in production code paths

**What to look for:** Commented-out logic, early returns with fake data, handlers that never call DB or integrations.

#### 3.3 Data and Persistence

- [ ] **DB access is real:** Reads/writes use the actual schema and migrations (or document which tables are “planned but not migrated”)
- [ ] **Migrations are consistent:** No obvious mismatch between schema and migrations; migrations are idempotent where required
- [ ] **Transactions where needed:** Multi-step writes use transactions if the stack supports it and the domain requires it

**What to look for:** Hardcoded arrays instead of DB, missing migrations for new tables/columns, orphaned migration files.

#### 3.4 API Contract and Invocation

- [ ] **Routes are reachable:** Mount path + method + path params match how clients (or docs) call them
- [ ] **Request/response shapes:** Request body/query/params and response body align with consumers (or OpenAPI/tRPC schema)
- [ ] **Errors are explicit:** Validation errors, 404, 403, 500 have consistent shape and status codes

**What to look for:** Wrong HTTP methods, missing or inconsistent error bodies, undocumented query/body params.

#### 3.5 Security and Access Control

- [ ] **Auth is enforced** on routes that require it
- [ ] **Authorization is enforced:** Role/permission checks run before performing the action
- [ ] **No sensitive data in logs or responses:** No passwords, tokens, or PII in plain text in logs or error messages

**What to look for:** Routes that should be protected but aren’t, missing permission checks, over-exposure of internal details.

#### 3.6 Error Handling and Resilience

- [ ] **Failures are handled:** Uncaught exceptions don’t crash the process without a top-level handler (or process manager)
- [ ] **External calls:** Timeouts, retries, or fallbacks where appropriate for third-party APIs
- [ ] **Database errors:** Connection failures and constraint violations are caught and converted to safe responses or logs

**What to look for:** Unhandled promise rejections, no timeouts on outbound calls, raw DB errors returned to client.

#### 3.7 Process Completeness

For each **process** (from Step 2), ask:

- [ ] **End-to-end implemented:** From “client request” to “persisted side effect / response” without gaps
- [ ] **Business rules implemented:** Validations, limits, and eligibility checks exist in code, not only in docs
- [ ] **Dependencies available:** Required env vars, DB tables, and external services exist or are clearly marked “not yet available”

**What to look for:** Processes that are “half built” (e.g. route exists but service is stub), missing validations, or reliance on “to-be-implemented” dependencies.

---

### Step 4: Classify as Working / Not Working / Needing Implementation

Using the examination results from Step 3, assign each **component** and each **process** to exactly one of:

#### Working

- **Definition:** The component or process is present, wired, and behaves as intended in the current system. No stubs, no known bugs that block core behavior.
- **Use when:** Implementation exists, is connected, and is used in the real flow; any gaps are minor (e.g. logging, edge cases) and do not change the “as-is” verdict.

#### Not Working

- **Definition:** The component or process exists and is wired, but it is broken, incorrect, or insecure (e.g. wrong permission check, crashes, returns wrong data, or violates a documented rule).
- **Use when:** Code is in place but behavior is wrong or unsafe; fixing it does not require “implementing from scratch” but rather correcting logic, wiring, or data handling.

#### Needing to be Implemented

- **Definition:** The component or process is missing, or is only stubbed/placeholder. To reach “working” state, implementation work is required (new code, new routes, new migrations, or new integrations).
- **Use when:** There is no implementation, or the implementation is explicitly temporary (TODO, stub, “return empty”) and the desired behavior is specified elsewhere (product spec, screen examination, migration plan).

**Optional severity for “Not Working” and “Needing Implementation”:**

- **Critical:** Blocks core user flows or security (e.g. auth broken, payment not persisted).
- **High:** Major feature broken or missing (e.g. RSVP not persisted, deal redemption missing).
- **Medium:** Secondary feature or robustness (e.g. fallback behavior, rate-limit bypass).
- **Low:** Minor or cosmetic (e.g. log format, minor validation).

**Output:** For each inventory row and each process, add a **Classification** column: `Working` | `Not Working` | `Needing Implementation`, and optionally **Severity** for non-Working items.

---

### Step 5: Produce the Structured Outline (Report)

Turn the inventory and classifications into a single **Backend Examination Report** with the following structure. This is the “structured outline of the system as-is.”

#### 5.1 Report Location and Naming

- **Path:** `docs/08-status/backend-examination/{DATE}_BACKEND_EXAMINATION_REPORT.md`
- **Naming:** `YYYY-MM-DD` = date of examination. One report per run.

#### 5.2 Required Sections

Use this template:

```markdown
# Backend Examination Report

**Date:** YYYY-MM-DD
**Examiner:** [AI Agent / named examiner]
**Repository:** [repository name]
**Scope:** [Full backend / bounded area]
**Process:** Per `docs/06-operations/examination/BACKEND_EXAMINATION_SOP.md`

---

## 1. Executive Summary

- **Components inventoried:** X
- **Processes identified:** Y
- **Working:** Z components, A processes
- **Not working:** B components, C processes
- **Needing implementation:** D components, E processes

**Key findings:** [2–4 bullet summary]

---

## 2. System as-Is: Component Inventory

[Table or list of every component from Step 1, with Type, Path, Classification, and optionally Severity/Notes.]

| # | Component | Path | Type | Classification | Severity | Notes |
|---|-----------|------|------|-----------------|----------|-------|
| 1 | Server entry | backend/hono.ts | Entry | Working | — | |
| 2 | Auth routes | backend/routes/auth.ts | Routes | Working | — | |
| … | … | … | … | … | … | … |

---

## 3. System as-Is: Process Map

[For each process from Step 2, list sub-flows and classification.]

### 3.1 Authentication and Authorization

| Sub-flow | Classification | Severity | Notes |
|----------|----------------|----------|-------|
| Login | Working | — | |
| Registration | Working | — | |
| OAuth (Google, etc.) | Working | — | |
| Session/token handling | Working | — | |
| Authorization (middleware) | Working | — | |

### 3.2 [Domain] (e.g. Events, News, Polls, …)

| Sub-flow / area | Classification | Severity | Notes |
|------------------|----------------|----------|-------|
| Create event | Working | — | |
| List events | Working | — | |
| RSVP | Needing implementation | High | No route/handler; screen examination expects POST /api/events/:id/rsvp |

### 3.3 Cross-Cutting

| Area | Classification | Severity | Notes |
|------|----------------|----------|-------|
| Error handling | Working | — | |
| Rate limiting | Working | — | |
| … | … | … | … |

---

## 4. Working

**Components:** [List or table of components classified Working.]

**Processes:** [List or table of processes/sub-flows classified Working.]

Use this section as the positive baseline: “these parts of the system are in place and functioning as-is.”

---

## 5. Not Working

**Components:** [List with Severity and short description.]

**Processes:** [List with Severity and short description.]

For each, add:
- **Location:** File and area
- **Issue:** What is wrong
- **Impact:** What breaks or is at risk
- **Recommendation:** Fix (code change, config change, etc.)

---

## 6. Needing to be Implemented

**Components:** [List with Severity and short description.]

**Processes:** [List with Severity and short description.]

For each, add:
- **Expected contract or behavior:** From API spec, screen examination, or product doc
- **Blockers:** Missing tables, env, or external service
- **Recommendation:** Implementation approach (route + service + migration, etc.)

---

## 7. Recommendations

1. [Immediate – Critical] …
2. [Short-term – High] …
3. [Medium-term – Medium] …

---

## 8. Next Steps

- [ ] Create or update feature requests for “Not working” and “Needing implementation” items
- [ ] Align backend report with screen examination report (e.g. “screen expects X, backend has Y”)
- [ ] Update architecture/behavior docs with as-is state
- [ ] Prioritize implementation order for migration or product backlog

---

**Last Updated:** YYYY-MM-DD
**SOP:** `docs/06-operations/examination/BACKEND_EXAMINATION_SOP.md`
```

---

### Step 6: Verify and Cross-Reference

- [ ] **Feature requests:** Ensure each “Not working” and “Needing implementation” item is either tracked in the GitHub Project (Feature Requests view, label `coordination:feature-request`) or has a clear “to be added” note
- [ ] **Screen examination:** Cross-check with the latest Screen Examination Report — e.g. “Screen expects POST /api/events/:id/rsvp” vs “Backend: RSVP = Needing implementation”
- [ ] **Architecture/behavior docs:** If `docs/03-architecture/` or `docs/04-behavior/` exist, add or update an “As-of YYYY-MM-DD backend” section or link to this report
- [ ] **Migration/executive overview:** If a Migration Executive Overview or Migration Process doc exists, reference this backend report where it describes “source backend state” or “gaps to implement in adapted system”

---

## Classification Quick Reference

| Classification | Meaning | Example |
|----------------|---------|---------|
| **Working** | Implemented, wired, and behaving as intended | Auth login, events list, polls vote |
| **Not working** | Exists but is broken or incorrect | Auth returns 500; deals route returns mock instead of DB |
| **Needing implementation** | Missing or stub-only | No RSVP endpoint; no deal redemption endpoint |

---

## Integration with Other Processes

- **Screen Examination SOP:** Screen report defines “what the UI expects from the API”; backend report defines “what the API provides.” Compare them to get a gap list (e.g. “UI calls X, backend has no X” → Needing implementation).
- **Migration Process:** Phase 1 (Discover) can use this SOP to document the **source** backend as-is. Phase 2 (Document) can store the resulting report in sourceapp and reference it in specs.
- **Documentation Process:** Treat the Backend Examination Report as a “subsystem doc” for the backend; update it when major backend changes are made, or link to the latest report from `docs/03-architecture/`.
- **Feature requests:** Items under “Not working” or “Needing implementation” should appear in Project Feature Requests view (label `coordination:feature-request`) with a link to this report.

---

## Notes

- **Don’t fix during examination:** Document first; changes happen in implementation or migration work, not inside the examination run.
- **Source is read-only:** When the backend is the migration source (e.g. gop_app), treat it as read-only; the report is “as-is” for planning only.
- **Structured outline:** Sections 4 (Working), 5 (Not Working), and 6 (Needing to be Implemented) are the core “structured outline” the user asked for; keep them concise and scannable.

---

**Last Updated:** 2026-01-27
