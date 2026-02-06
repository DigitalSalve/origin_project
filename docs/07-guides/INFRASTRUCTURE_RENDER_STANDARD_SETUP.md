# Infrastructure: Render — Standard Setup

**Purpose:** This document and its template define the **standard Render deployment setup** carried from project to project. The layout uses a proven base (static + API + worker + database): Vite-style SPA at repo root for the static web, Node API and worker, Postgres. It aligns with the [Infrastructure: Authentication — Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md). Build off this structure; adapt build/start commands and env vars per project.

**Use when:** You are standing up a new project on Render or migrating an existing one. Copy the template, replace the project slug, adapt build/start commands and env vars per project, and deploy.

**Template file:** [render.yaml.template](templates/render.yaml.template). Copy to repo root as `render.yaml` and replace every occurrence of `myproject` with the project slug from [PROJECT_IDENTITY](../01-orientation/PROJECT_IDENTITY.md).

---

## 1. Standard stack (four units)

| Unit | Type | Purpose |
|------|------|---------|
| **&lt;slug&gt;-web** | Static site | Web app build (e.g. SPA, Flutter web). SPA rewrite; env for API base URL. |
| **&lt;slug&gt;-api** | Web service | API server. Session auth, magic-link, CORS, app identifiers. Uses `SESSION_SIGNING_KEY`, SendGrid, `DATABASE_URL` from DB. See [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §3.6. |
| **&lt;slug&gt;-worker** | Worker | Background jobs (e.g. magic-link email, notifications). No `SESSION_SIGNING_KEY`; has `DATABASE_URL` and email/queue credentials. |
| **&lt;slug&gt;-db** | Database | PostgreSQL. API and worker get `DATABASE_URL` via `fromDatabase`. |

*&lt;slug&gt;* = project slug from PROJECT_IDENTITY (e.g. `sourceapp`).

---

## 2. Service details

### 2.1 Web (static)

The template uses a **proven base**: Vite/React-style SPA at repo root, clean install and build, output to `dist/`, SPA rewrite, one env var for the API URL.

- **Build (default):** `rm -rf node_modules package-lock.json && npm install && npm run build` — clean install and full build each deploy. Root `package.json` should have `"build": "vite build"` (or equivalent) and output to `dist/`.
- **Publish path:** `dist` — where the build writes assets.
- **Routes:** SPA rewrite `/*` → `/index.html` so client-side routing works.
- **Env:** `VITE_API_BASE_URL` — set to the API service URL so the built app knows the backend. (Vite exposes env vars prefixed with `VITE_` at build time. For non-Vite bundlers, use `API_BASE_URL` or equivalent and adapt the template.)

**Other frontends:** For Flutter web or other stacks, change `buildCommand` and `staticPublishPath` (e.g. `flutter build web` and `build/web`). Use Docker or Render-native support if the stack is not Node.

### 2.2 API (web service)

- **Build/start:** Node (or other runtime) — e.g. `npm install`, `npm run start` or `node server.js`.
- **Health:** Set `healthCheckPath: /health` and expose a `/health` route.
- **Env (required for core auth):** `SESSION_SIGNING_KEY`, `DATABASE_URL` (from DB), SendGrid (`SENDGRID_API_KEY`, `SENDGRID_FROM_EMAIL`), `CORS_ORIGINS`, `ALLOW_LOCALHOST_ORIGINS`. App identifiers for deep links: `APP_BUNDLE_ID`, `ANDROID_PACKAGE_NAME`, `APPLE_TEAM_ID` (and `ANDROID_*_SHA256` if using app links). See [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §3.6.
- **Env (project-specific):** OAuth, payment, geocoding, etc. Add only what the project uses.

### 2.3 Worker

- **Build/start:** Same repo as API; e.g. `npm install`, `npm run start:worker`. Worker code must not issue or validate sessions.
- **Env:** `DATABASE_URL` (from DB). Email/queue credentials (e.g. `SENDGRID_*`) for async jobs. **Do not** set `SESSION_SIGNING_KEY`. See [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §5.
- **When to add:** Use from day one if the project does magic-link or other async email/notification; otherwise add when the first background job exists.

### 2.4 Database

- **Render:** `databases:` block, `plan: basic-1gb` (or other plan). Name = `&lt;slug&gt;-db`.
- **Connection:** API and worker reference it with `fromDatabase: name: <slug>-db, property: connectionString`.

---

## 3. Implementation checklist

| Step | Action |
|------|--------|
| **Template** | Copy [render.yaml.template](templates/render.yaml.template) to repo root as `render.yaml`. |
| **Slug** | Replace every `myproject` with the project slug from PROJECT_IDENTITY. |
| **Web** | Default: clean install + `npm run build`, `dist/`, `VITE_API_BASE_URL`. For Flutter or other stacks, change `buildCommand`, `staticPublishPath`, and env var. |
| **API** | Set `buildCommand`, `startCommand`, `healthCheckPath`. Add env vars per §2.2 and [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §3.6. |
| **Worker** | Set `startCommand` (e.g. `npm run start:worker`). Add only DB + email/queue env; no `SESSION_SIGNING_KEY`. Stub or omit if no jobs yet. |
| **DB** | Keep `fromDatabase` and DB name aligned with `&lt;slug&gt;-db`. |
| **Secrets** | Set all `sync: false` (or secret refs) in Render dashboard; never commit secrets. |

---

## 4. Per-project adaptation

- **Project slug:** Only place that must be project-specific in the template is the slug (service and DB names). Use PROJECT_IDENTITY.
- **Web build:** Default is Vite-style SPA at repo root (clean install + build → `dist/`, `VITE_API_BASE_URL`). For Flutter or other frontends, change `buildCommand`, `staticPublishPath`, and the API-URL env var.
- **API/worker stack:** Template assumes Node; change `env` and commands if the project uses another runtime.
- **Env vars:** Add or remove API/worker env vars to match the project (OAuth, payments, integrations). Keep the rules: API has `SESSION_SIGNING_KEY` and auth/email/CORS/app-id vars; worker does not have `SESSION_SIGNING_KEY`.

---

## 5. Relation to other core infra

- **Auth:** [Infrastructure: Authentication — Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) defines session/magic-link, API vs worker env, and DB usage. This Render setup supplies the four units that run that pattern.
- **Bootstrap:** When creating a new project from this repo, copy this doc and [render.yaml.template](templates/render.yaml.template) as part of infrastructure. See [Bootstrap New Project](../01-orientation/BOOTSTRAP_NEW_PROJECT.md).

---

## 6. Replication coverage — can we replicate the working base?

**Yes.** The core infra (this doc + [render.yaml.template](templates/render.yaml.template) + [Infrastructure: Authentication — Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md)) gives you everything needed to replicate the same working structure. Use this as a checklist.

| What needs to work | Where it lives in core infra |
|--------------------|-------------------------------|
| **Four units on Render** (static + API + worker + DB) | This doc §1; [render.yaml.template](templates/render.yaml.template). Copy template, replace slug, deploy. |
| **Static web** — clean install, build → dist, SPA rewrite, API URL at build time | Template: `buildCommand`, `staticPublishPath: dist`, routes `/*` → `/index.html`, `VITE_API_BASE_URL`. This doc §2.1. |
| **API** — Node, health check, DB from Render, session/auth env | Template: API service, `fromDatabase`, `healthCheckPath: /health`. Env: `SESSION_SIGNING_KEY`, SendGrid, CORS, app ids. This doc §2.2; [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §3.6. |
| **Worker** — Node, DB from Render, email/queue env, no session key | Template: worker service, `fromDatabase`, `start:worker`. Env: `DATABASE_URL`, SendGrid only; no `SESSION_SIGNING_KEY`. This doc §2.3; [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §5. |
| **Database** — Postgres, API and worker use `fromDatabase` | Template: `databases` block, `myproject-db`. This doc §2.4. |
| **Auth model** — magic-link + backup code, web cookie, mobile Bearer/device, trusted devices | [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §§1–6: flows, routes, headers, cookie names, DB tables/migrations, implementation checklist. |
| **Auth API routes** — web and mobile auth + device APIs | [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §3.1–3.2: full route list. |
| **Auth DB** — users, magic_links, auth_sessions, trusted_devices, devices, migration order | [Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) §4.1–4.2. |

**What you add per project:** Implement the auth and worker logic (following the Core Flow spec), run the DB migrations in the order given, and add any project-specific integrations (payments, identity, etc.) and their env vars. The core infra is the shared bones; you replicate by following it and filling in project-specific code and secrets.

---

**Last updated:** 2026-01-27
