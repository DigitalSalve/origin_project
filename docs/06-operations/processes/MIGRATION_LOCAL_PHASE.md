# Migration: Work in Isolation of Render

**Purpose:** Define migration work that can be done **without** a Render account or deployed environment. Use this to progress on the migration using only local tooling (local Postgres, Neon, Docker, etc.) and repo changes.

**Process:** Per [Migration Process](MIGRATION_PROCESS.md). All adaptation work is in the sourceapp repo; source (gop_app) is read-only.

**When to use:** Before or in parallel with connecting to Render. Run through the checklist below so API, worker, DB, and deploy config are ready; deploy to Render when the environment is available.

---

## 1. What “in isolation of Render” means

- **No Render account or deploy** — No `render.com` login, no Blueprint deploy, no Render dashboard.
- **Local or external-only** — Backend runs locally (Node/Bun), DB is local Postgres or Neon (or other external Postgres), worker runs locally or is stubbed.
- **Repo-only deliverables** — New or adapted code, migrations, config files, and docs live in the sourceapp repo and are runnable/testable without Render.

---

## 2. Work items (checklist)

Do these in an order that respects dependencies. Completion criteria are per item.

| # | Work item | Completion criterion | Blocks / notes | Status |
|---|-----------|----------------------|----------------|--------|
| 1 | **render.yaml in repo** | `render.yaml` at sourceapp repo root, built from [render.yaml.template](../../07-guides/templates/render.yaml.template) with slug = sourceapp. Ready for when Render is connected. | None. | **Done** (2026-01-27) |
| 2 | **Local run instructions** | README or runbook in sourceapp that describes how to run API + worker + DB locally (e.g. `npm run start`, `npm run start:worker`, local Postgres or Neon `DATABASE_URL`). | Needed before meaningful backend work. | |
| 3 | **Adapted backend in sourceapp** | Backend code (API entry, routes, middleware, DB client) lives under sourceapp (e.g. `backend/` or `apps/api/`), adapted from gop_app. Runnable locally. | Depends on run instructions; ref. gop_app read-only. | |
| 4 | **API health route** | `GET /health` implemented and returns 200 when the app and DB are up. | Required by [Render standard setup](../../07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md); do with backend adaptation. | |
| 5 | **Worker entry** | `npm run start:worker` (or equivalent) starts a worker process. Can be a stub that exits or polls no-ops until real jobs exist. | Required by render.yaml; do with backend adaptation or minimal new module. | |
| 6 | **Core-auth DB migrations** | If adopting [Infrastructure: Authentication — Core Flow](../../07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md), add migrations for users, magic_links, auth_sessions, trusted_devices, devices in the order in that doc §4.1. Run locally. | Depends on adapted backend and local DB. | |
| 7 | **Env / secrets template** | `.env.example` or `env.example` in sourceapp listing all env vars required for API and worker (from [Core Flow](../../07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §3.6 and [Render standard setup](../../07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md) §2). No real secrets; used for local and later Render. | None. | |
| 8 | **Static web build (if applicable)** | If sourceapp has a web app (Vite or Flutter web), ensure `npm run build` / `flutter build web` works and output path is documented for render.yaml (`dist/` or `build/web`). | **Done** — React (marketing/) → `out/`; Flutter → `build/web`; both in render.yaml (sourceapp-web, sourceapp-webapp). | |

---

## 3. Ordering and dependencies

- **First:** (1) render.yaml, (7) env example — no runtime required.
- **Then:** (2) local run instructions — requires a runnable backend; can be drafted for gop_app layout and updated when adapted backend exists.
- **Then:** (3) adapted backend, (4) health route, (5) worker entry — backend work in sourceapp; runnable locally.
- **Then:** (6) core-auth migrations — once DB and backend exist in sourceapp.
- **When needed:** (8) static web build — when we are ready to deploy the web service.

---

## 4. Relation to other docs

- **Migration Process:** [MIGRATION_PROCESS.md](MIGRATION_PROCESS.md) — phases Discover → Document → Plan → Implement → Verify. This doc is a **slice** of Implement: work that does not require Render.
- **Render standard setup:** [INFRASTRUCTURE_RENDER_STANDARD_SETUP.md](../../07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md) — once Render is used, render.yaml and env match that standard.
- **Core auth:** [INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md](../../07-guides/INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) — DB migrations and API/worker env for auth follow that doc.
- **Flutter / backend plan:** [FLUTTER_CONVERSION_PLAN.md](../../08-status/feature-requests/FLUTTER_CONVERSION_PLAN.md) — Backend Migration to Render and target stack; local phase prepares the same backend for local run and later Render.

---

## 5. Updating this checklist

- Add work items when new “in isolation” scope is agreed (e.g. auth API routes, worker job specs).
- Mark items done in the table (e.g. “Done”) and record completion in feature requests or SCOPE_OF_WORK as appropriate.
- When Render is in use, move deploy-specific work out of this list into the main migration plan or Render-runbook.

---

**Last updated:** 2026-01-27
