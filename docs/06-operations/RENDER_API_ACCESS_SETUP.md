# Render API Access Setup

**Purpose:** Enable programmatic access to your Render environment so an AI agent or scripts can query services, deploys, logs, and environment state. Use when you want direct API or CLI access to Render.

**When to read:** Setting up Render API access for automation, AI agents, or local tooling.

---

## How Cursor (AI agent) accesses Render

**Current state:** Cursor does **not** automatically access Render. It uses the following flow when you ask it to query Render:

1. **Prerequisite:** You must create `.env.render` at the repo root with `RENDER_API_KEY=your_key` (see Option 1 below).
2. **Mechanism:** The agent runs terminal commands that source `.env.render` and then call the Render API via `curl`:
   ```bash
   cd /path/to/sourceapp && source .env.render 2>/dev/null || export $(grep -v '^#' .env.render | xargs) && curl -s -H "Authorization: Bearer $RENDER_API_KEY" "https://api.render.com/v1/services?limit=5"
   ```
3. **If `.env.render` is missing or empty:** The Render API will return 401 Unauthorized. The agent will report that Render API access is not configured.

**No built-in integration:** Cursor has no native Render integration. Access is via `curl` + `RENDER_API_KEY` from `.env.render`.

---

## Options

| Method | Best for | Setup |
|--------|----------|-------|
| **API key** | AI agents, scripts, CI/CD | Create key in dashboard → store in `.env.render` |
| **Render CLI** | Interactive + non-interactive | `npm install -g render` or `brew install render` |

**Recommendation for AI agents:** Use the **API key** approach. The agent can read `RENDER_API_KEY` from a gitignored `.env.render` file and call the Render API directly via `curl` or HTTP.

---

## Option 1: API Key (Recommended for Agents)

### 1. Create an API key

1. Go to [Render Dashboard → Account Settings](https://dashboard.render.com/u/settings)
2. Open the **API Keys** section
3. Click **Create API Key**
4. Name it (e.g. `sourceapp-agent` or `cursor-agent`)
5. **Copy the key immediately** — it is shown only once. If lost, revoke and create a new one.

### 2. Store the key securely

Create a file `.env.render` in the **repository root** (same level as `render.yaml`):

```
RENDER_API_KEY=rnd_xxxxxxxxxxxxxxxxxxxxxxxx
```

**Security:**
- Never commit `.env.render` to version control
- `.env.render` is listed in `.gitignore`
- If the key is compromised, revoke it in the dashboard and create a new one

### 3. Verify access

From the project root (with `RENDER_API_KEY` set), run:

```bash
source .env.render 2>/dev/null || export $(grep -v '^#' .env.render | xargs)
curl -s -H "Authorization: Bearer $RENDER_API_KEY" \
  "https://api.render.com/v1/services?limit=5" | head -100
```

You should see JSON listing your services (e.g. `sourceapp-web`, `sourceapp-webapp`, `sourceapp-api`, `sourceapp-worker`, `sourceapp-db`).

### 4. What the agent can do with the API

With `RENDER_API_KEY` available, the agent can:

| Action | Endpoint / Method |
|--------|-------------------|
| List services | `GET /v1/services` |
| Get service details | `GET /v1/services/{id}` |
| List deploys | `GET /v1/services/{id}/deploys` |
| Get deploy logs | `GET /v1/deploys/{id}/logs` |
| List environment variables | `GET /v1/services/{id}/env-vars` |
| Trigger deploy | `POST /v1/services/{id}/deploys` |
| List datastores | `GET /v1/datastores` |
| List workspaces | `GET /v1/owners` |

**Reference:** [Render API Reference](https://api-docs.render.com/reference)

---

## Option 2: Render CLI

### 1. Install

```bash
# npm (global)
npm install -g render

# or Homebrew (macOS)
brew install render
```

### 2. Authenticate

```bash
render login
```

This opens a browser to authenticate. For non-interactive use (scripts, CI), you can use `RENDER_API_KEY`:

```bash
export RENDER_API_KEY=rnd_xxxxxxxxxxxxxxxxxxxxxxxx
render services list
```

### 3. Common commands

| Command | Purpose |
|---------|---------|
| `render services list` | List services |
| `render deploys list --service <id>` | List deploys for a service |
| `render logs <service>` | Stream logs |

**Reference:** [Render CLI docs](https://render.com/docs/cli)

---

## Option 3: Render Dashboard (API service pattern)

**Use case:** The **deployed API service** needs to call the Render API (e.g. to fetch logs for admins). The key is stored on Render, not locally.

**How it works:**
- `RENDER_API_KEY` is set as an **environment variable** on the API service (e.g. `sourceapp-api`) in the Render Dashboard
- Not in `render.yaml` (secrets use `sync: false` and are configured manually)
- The backend reads `process.env.RENDER_API_KEY` when admins request logs via `GET /api/admin/render-logs/:serviceName`
- The API then calls `https://api.render.com/v1/{service_type}/{service_id}/logs` with `Authorization: Bearer ${renderApiKey}`

**Setup:**
1. Create API key in [Render Dashboard → Account Settings](https://dashboard.render.com/u/settings)
2. Go to your API service (e.g. `sourceapp-api`) in Render Dashboard
3. Environment → Add variable: `RENDER_API_KEY` = your key (mark as secret)
4. Redeploy if needed

**Also used:** `RENDER_WEBHOOK_SECRET` for webhook verification when Render sends service/deploy status updates.

---

## Blueprint sync (Dashboard only)

**Note:** Syncing a Blueprint (to create or update services from `render.yaml`) is done from the Render dashboard. There is no dedicated "sync blueprint" API endpoint. When you push changes to `render.yaml`, Render may auto-sync if the repo is connected; otherwise, use the dashboard to manually sync the Blueprint.

---

## Quick reference

| Task | How |
|------|-----|
| Give agent API access | Create API key → put in `.env.render` → ensure `.env.render` is gitignored |
| Test API key | `curl -H "Authorization: Bearer $RENDER_API_KEY" https://api.render.com/v1/services?limit=3` |
| Use CLI | `render login` or `export RENDER_API_KEY=...` then `render services list` |

---

**Related:** [Deployment Workflow](DEPLOYMENT_WORKFLOW.md) · [Infrastructure: Render — Standard Setup](../07-guides/INFRASTRUCTURE_RENDER_STANDARD_SETUP.md)
