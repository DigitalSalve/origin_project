# Deployment Workflow

**Purpose:** Defines how SourceApp is deployed. There is **no local deployment** — all deployment happens via GitHub → Render auto-sync.

**Last Updated:** 2026-01-28

---

## Before You Push (Mandatory)

**Every push triggers a full rebuild on Render.** Failed deployments consume build time and cost without delivering value.

**Rule:** Do not push backend or worker changes until production validation passes. Run:

```bash
cd backend && npm run validate:production
```

This script simulates Render's environment (`npm install --omit=dev`, then `npm run start`), verifies the API starts and responds to `/health`, then restores devDependencies. If it fails, fix before pushing.

**When to run:**
- Before every push that touches `backend/` (API, worker, package.json, render.yaml)
- Before first deployment of a new backend/worker service

**Cost rationale:** A failed Render build wastes build minutes. Running `validate:production` locally (seconds) prevents wasted Render builds. See [Production Deployment Validation SOP](coordination/PRODUCTION_DEPLOYMENT_VALIDATION_SOP.md).

---

## Production Readiness

**Before deploying backend or worker changes**, run the [Production Deployment Validation SOP](coordination/PRODUCTION_DEPLOYMENT_VALIDATION_SOP.md). This prevents deployment failures caused by configuration mismatches (e.g. runtime tools in devDependencies). See [Render Production Environment](#render-production-environment) below.

---

## Workflow Overview

```
Local development → Validate (backend) → Commit → Push to GitHub → Render auto-deploys
```

| Step | Where | What happens |
|------|-------|--------------|
| **Develop** | Local machine | Edit code, run locally for testing |
| **Validate** | Local machine | `cd backend && npm run validate:production` — simulates Render; **required before push if backend changed** |
| **Commit** | Local machine | `git add`, `git commit` |
| **Push** | Local → GitHub | `git push` to GitHub |
| **Deploy** | Render | Render auto-syncs from GitHub and deploys |

**No manual deployment to Render.** Render is connected to the GitHub repo and deploys automatically when you push. **Validate before push to avoid wasting Render build time on failing deployments.**

---

## What Gets Deployed

When you push to GitHub, Render deploys:

| Service | Source | Purpose |
|---------|--------|---------|
| **sourceapp-web** | React/Next.js build from `marketing/` | Public web (sourcehub.example). Root Directory = marketing. |
| **sourceapp-webapp** | Flutter web build at repo root | Member app web (app.sourcehub.example). Root Directory = blank. |
| **sourceapp-api** | `backend/` directory | API server |
| **sourceapp-worker** | `backend/` directory | Background jobs |
| **sourceapp-db** | Render PostgreSQL | Migrations run separately (see below) |

---

## First-time Render setup (checklist)

After connecting the repo to Render (Blueprint or manual services), do this once so both web services and API work:

1. **sourceapp-web (React website)** — In Render dashboard: **sourceapp-web** → **Settings** → **Root Directory** = `marketing` (required). If blank, Render builds from repo root and you get the Flutter app on sourcehub.example. Trigger a new deploy after changing.
2. **sourceapp-webapp (Flutter app)** — **sourceapp-webapp** → **Settings** → **Root Directory** = blank (repo root). Set **API_BASE_URL** env var (e.g. `https://sourceapp-api.onrender.com`).
3. **Custom domains** — Attach **sourcehub.example** to **sourceapp-web** and **app.sourcehub.example** to **sourceapp-webapp** (Dashboard → Service → Settings → Custom Domains).
4. **sourceapp-api CORS** — Set **CORS_ORIGINS** to include `https://sourcehub.example` and `https://app.sourcehub.example` (and Render preview URLs if needed).

Full checklist and open decisions: [Two Web Services — Architecture Plan](../08-status/TWO_WEB_SERVICES_ARCHITECTURE_PLAN.md) §4.

---

## Local vs Deployed

| Activity | Local | Render |
|----------|-------|--------|
| **Run API** | `cd backend && npm run start` (for testing) | Auto-deployed from GitHub |
| **Run React marketing** | `cd marketing && npm run dev` (for testing) | Deploy: **sourceapp-web** |
| **Run Flutter web (member app)** | `flutter run -d chrome` (for testing) | Auto-deployed from GitHub (**sourceapp-webapp**) |
| **Database** | Optional: local Postgres or Neon for dev | Render PostgreSQL (sourceapp-db) |
| **Deploy** | N/A — no local deploy | Push to GitHub |

---

## Database Migrations

Database migrations are **not** auto-run on push. Options:

1. **Manual (recommended):** Run migrations from local when needed, pointing at the Render database:
   ```bash
   cd backend
   DATABASE_URL="<paste connection string from Render sourceapp-db → Connect → External connection string>" npm run db:migrate
   ```
   Or use `npm run db:push` if your workflow uses schema push instead of migration files. Get the connection string from Render Dashboard → sourceapp-db → **Connect** → **External connection string** (use the same URL in `.env` for local runs against Render DB; never commit it).

   **Note for Render Postgres:** This repo’s `backend/db/migrations/` contains **raw SQL files** (no Drizzle `meta/_journal.json`). `npm run db:migrate` fails with “Can’t find meta/_journal.json”. `npm run db:push` uses the Neon serverless driver (websocket) and does not complete against Render Postgres (TCP). To apply migrations against Render: run the SQL files in **filename order** with `psql`, e.g. `psql "$DATABASE_URL" -f db/migrations/20250220_add_deal_redemption_limits.sql` (and so on for each file), or add a Drizzle journal and a TCP-compatible driver for `drizzle-kit migrate`.
2. **Render Release Command (optional):** To run migrations on each deploy, add a **Release Command** for **sourceapp-api** in Render (e.g. `npm run db:migrate`). That requires `drizzle-kit` in **dependencies** (not devDependencies) so it is installed in production. See [DATABASE_MIGRATIONS_SOP](processes/DATABASE_MIGRATIONS_SOP.md).
3. **Document:** Migration scripts and usage are in `backend/README.md`.

---

## Environment Variables

- **Local:** Use `.env` (from `.env.example`) for local development. Never commit `.env`.
- **Render:** Set in Render dashboard per service (or in **grouped env vars** attached to each service). `render.yaml` defines which vars each service needs; `sync: false` means you set them manually in the dashboard.
- **List by service/group:** See [ENV_VARS_RENDER.md](ENV_VARS_RENDER.md) for a full list of variables to add and **where** (sourceapp-api, sourceapp-worker, sourceapp-webapp), including required vs optional.

---

## Render Production Environment

Render runs Node.js services with `NODE_ENV=production`. This affects dependency installation:

| Behavior | Local (dev) | Render (production) |
|----------|-------------|----------------------|
| **npm install** | Installs all deps (including devDependencies) | Runs `npm install --omit=dev` — **devDependencies are NOT installed** |
| **Start command** | Uses `package.json` `start` or `start:worker` | Same; but any tool invoked must be in `dependencies` |

**Critical rule:** If the start command invokes a tool (e.g. `tsx`, `npx`, a CLI), that tool's package must be in `dependencies`, not `devDependencies`. Otherwise Render will fail with "command not found" or "Cannot find module".

**Validation:** Run `npm install --omit=dev` locally, then `npm run start`. If it fails, fix before pushing. See [Production Deployment Validation SOP](coordination/PRODUCTION_DEPLOYMENT_VALIDATION_SOP.md).

---

## Blueprint sync and cache (troubleshooting)

**Blueprint sync vs service deploy:** They are different. **Blueprint sync** (Blueprint → Syncs) applies `render.yaml` to the stack (create/update services and config). **Service deploy** (each service → Events) builds and deploys that service on push. So you can see “Deploy live” for a service while Blueprint Syncs still show failures — the repo push triggers builds, but applying the Blueprint (the IaC definition) may be failing.

**Static / web service missing when creating or syncing Blueprint:** If the Blueprint only shows database + API + worker (3 services) and the static site (e.g. sourceapp-web or sourceapp-webapp) is missing, Render’s parser may be dropping the first service when it has a multiline `buildCommand: |`. Use a single-line `buildCommand` (commands chained with `&&`) for that service so the YAML parses correctly. See `render.yaml` in this repo for the pattern.

**sourceapp-webapp Flutter build: "destination path already exists":** If the build fails with `fatal: destination path '/opt/render/flutter' already exists and is not an empty directory`, the Flutter SDK clone is hitting a non-clean build environment. The `buildCommand` in `render.yaml` must remove the directory before cloning: start with `rm -rf $HOME/flutter &&` before the `git clone` step so each build gets a clean Flutter install.

**Both sourceapp-web and sourceapp-webapp show the same UI (e.g. both show Flutter / red header):** The marketing service is building from the wrong directory. In Render dashboard, open **sourceapp-web** → **Settings** → ensure **Root Directory** is set to `marketing` (not blank). If it is blank, Render builds from repo root (Flutter), so you get the Flutter app on both. After setting Root Directory to `marketing`, trigger a new deploy. **sourceapp-web** should then serve the React site (dark blue header, “Dream Big. Live Proudly. Cherokee County.”). **sourceapp-webapp** must have Root Directory blank (repo root) so it builds Flutter.

**Blueprint sync failing (red X on Syncs):** Get the actual error: in the dashboard open the Blueprint → **Syncs**, click **View details** on a failed sync to see why it failed. Common causes:

- **Invalid or deprecated keys in `render.yaml`** — Use `runtime` (e.g. `runtime: static`, `runtime: node`), not deprecated `env`. Validate locally: `render blueprints validate render.yaml` (Render CLI v2.7.0+), or use the [Validate Blueprint API](https://api-docs.render.com/reference/validate-blueprint).
- **Immutable fields on existing services** — If services were created with an old spec (e.g. `env`), changing to `runtime` in `render.yaml` may still fail on sync because Render does not allow changing type/runtime on existing services. In that case you may need to create a new Blueprint or recreate the affected services so they are created with the current `render.yaml`.

**Blueprint not syncing / not picking up changes:** Render reads `render.yaml` from your linked repo. If the Blueprint isn’t applying:

1. **Force Render to re-read the repo** — Push a change so Render sees a new commit and re-syncs the Blueprint:
   - Append an entry to [Manual Sync Log](../08-status/MANUAL_SYNC_LOG.md) with Target **Render Blueprint** and Reason **Blueprint not syncing — force re-sync** (see [Manual Sync Trigger SOP](coordination/MANUAL_SYNC_TRIGGER_SOP.md)).
   - Commit and push. Render will see the new commit and can re-apply the Blueprint.
2. **Manual sync in Render Dashboard** — In the [Render Dashboard](https://dashboard.render.com), open your **Blueprint** (the one linked to this repo). If Render shows pending changes from `render.yaml`, click **Apply** to sync. If it doesn’t show changes, the repo link or branch may be wrong; check Blueprint settings.
3. **Clear build cache (per service)** — If a *service* is using stale build cache (not the Blueprint itself), use **Clear build cache & Deploy** for that service in the dashboard: Service → **Manual Deploy** → **Clear build cache & Deploy**. There is no `render.yaml` option to clear cache on every deploy; use the dashboard or the [Render API](https://api-docs.render.com) deploy endpoint with clear-cache if supported.

**Summary:** To fix Blueprint sync, push a repo change (e.g. log entry) so Render re-reads the repo, and/or use the dashboard to Apply Blueprint changes. To clear a service’s build cache, use the dashboard’s “Clear build cache & Deploy” for that service.

---

## Summary

- **Develop locally** — Run API, React marketing (cd marketing), and Flutter member app locally for testing
- **Commit and push** — All changes go to GitHub
- **Render auto-deploys** — No manual deploy step; Render syncs from GitHub and rebuilds/redeploys
