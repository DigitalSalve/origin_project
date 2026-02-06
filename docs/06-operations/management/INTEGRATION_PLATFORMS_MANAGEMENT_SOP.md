# Integration Platforms — Management SOP

**Purpose:** Defines how to manage integration-platform settings, update procedures, and documentation. Use when adding a new platform, changing env/secrets, rotating credentials, upgrading SDKs, or retiring a platform.

**Status:** Active

**Applies To:** Backend and platform engineers, DevOps, and AI agents adding or changing third-party integrations (SendGrid, OpenAI, Render, AWS, Cloudflare, Plaid, Ingo, Firebase, etc.).

---

## Overview

This SOP ensures that:

1. **Settings are documented and governed** — Env vars, API keys, and secret locations are recorded in the right place (Layer 3 catalog + Layer 5 provider docs); secrets are never stored in repo or in architecture docs.
2. **Update procedures are clear** — Rotation, region changes, SDK upgrades, and config changes follow a consistent process and are reflected in docs and runbooks.
3. **New platforms are added consistently** — Catalog (INTEGRATION_PLATFORMS.md), Layer 5 docs, and SYSTEM_MAP are updated together; ownership and “where to change what” are explicit.
4. **Information stays in the right layer** — Layer 3 holds the catalog and governance index; Layer 5 holds provider-specific implementation, env lists, and webhook contracts.

---

## Prerequisites

Before adding, changing, or retiring an integration platform:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - `docs/03-architecture/INTEGRATION_PLATFORMS.md` (catalog and categories)
   - [Integration Platforms Management SOP](INTEGRATION_PLATFORMS_MANAGEMENT_SOP.md) (this doc)
   - Any existing Layer 5 doc for the platform (e.g. `docs/05-integrations/PLAID_*.md`)

2. **Understand layers:**
   - **Layer 3 (INTEGRATION_PLATFORMS.md):** What platforms exist, their category, status, and pointer to “where settings live” and “detailed doc.” No secret values.
   - **Layer 5 (`docs/05-integrations/`):** Per-provider env vars, API contracts, webhook verification, upgrade/rotation steps. Still no raw secrets in repo; only names and examples.

3. **Identify change type:** New platform, settings/secret change, upgrade/rotation, or retirement. Use the matching process step below.

---

## Process

### Step 1: Add a New Integration Platform

Use when the project is adopting a new third-party service (e.g. adding OpenAI, Cloudflare, or a new email provider).

#### 1.1 Decide category and status

- [ ] **Choose category:** Email, AI/ML, Hosting/Runtime, Database, CDN/Edge, Payments/Identity, Push, Other.
- [ ] **Set status:** **Planned** until it is wired and used in at least one environment; then **Active**.
- [ ] **Confirm approval:** New integrations that affect canon, compliance, or money movement require explicit approval (feature request or design doc). Record decision and link.

#### 1.2 Update the architecture catalog

- [ ] **Open** `docs/03-architecture/INTEGRATION_PLATFORMS.md`.
- [ ] **Add one row** to the Catalog (Summary Table): Platform name, Category, Purpose (short), Status, Settings/env reference (e.g. `OPENAI_*`, “Render dashboard”), and “Detailed doc (Layer 5)” link or “TBD”.
- [ ] **Add or extend** the relevant category section under “Categories and Platform Notes” with 2–4 sentences: what it’s for, status, and where settings are documented (Layer 5).

#### 1.3 Create or update Layer 5 documentation

- [ ] **Create** a provider doc in `docs/05-integrations/` (e.g. `OPENAI_IMPLEMENTATION.md`, `CLOUDFLARE_IMPLEMENTATION.md`) or add to an existing file if it fits (e.g. “AWS”).
- [ ] **Include:** Purpose, required env var **names** (no values), example shape (e.g. `OPENAI_API_KEY=sk-...`), sandbox vs production where applicable, and link to official provider docs.
- [ ] **If the platform has webhooks:** Add or reference webhook verification and payload expectations (or point to `WEBHOOK_CONTRACTS.md`).

#### 1.4 Update SYSTEM_MAP and related docs

- [ ] **SYSTEM_MAP.md:** If the platform touches code paths (e.g. new config file, new worker usage), add a one-liner and link to the Layer 5 doc.
- [ ] **Behavior/API/UI docs:** If the integration affects APIs or UI (e.g. new endpoints, new env), update API_MATRIX, UI_INTEGRATION_POINTS, or behavior docs per their usage SOPs.

**Output:** New platform is in the catalog, has a Layer 5 doc, and SYSTEM_MAP (and any wiring docs) are aligned. No secrets in repo.

---

### Step 2: Manage Settings and Secrets

Use when adding, renaming, or rotating env vars, API keys, or other secrets for an existing platform.

#### 2.1 Document the change

- [ ] **Identify** the platform in `docs/03-architecture/INTEGRATION_PLATFORMS.md` and its Layer 5 doc.
- [ ] **Update Layer 5** with the correct env var **names** and purpose. Use placeholders or examples only (e.g. `SENDGRID_API_KEY=<secret>`); never commit real secrets.
- [ ] **If the catalog’s “Settings / env reference” is wrong** (e.g. new prefix), update the catalog row so it still points accurately.

#### 2.2 Apply in environments

- [ ] **Set values** in the appropriate secret store or platform env (e.g. Render env, AWS Secrets Manager, `.env` for local only). Follow project conventions for local vs staging vs production.
- [ ] **Rotation:** Plan downtime or overlap (e.g. dual-key rotation) per provider guidance; document the steps in the platform’s Layer 5 section or in a runbook and link from the catalog or SOP.

#### 2.3 Verify and record

- [ ] **Smoke-test** the integration after the change (e.g. send test email, call test endpoint).
- [ ] **Note** in the Layer 5 doc or a changelog when rotation/rename was done and by whom (or “see runbook link”).

**Output:** Settings are correct in code/env, docs show current env names and where to set them, and rotation/runbook steps are recorded. No secrets in repo.

---

### Step 3: Update Procedures (Upgrades, Region Changes, Config)

Use when upgrading an SDK, changing region/provider config, or changing how the app uses the platform (e.g. new API version, new region).

#### 3.1 Plan the change

- [ ] **Decide** what is changing: SDK version, API version, region, feature flag, or config (e.g. webhook URL, allowlist).
- [ ] **Check** provider changelog and migration notes; note breaking changes and backwards-compat windows.
- [ ] **Identify** all env vars, config files, and code paths that use this platform (use Layer 5 and SYSTEM_MAP).

#### 3.2 Document the procedure

- [ ] **Write or update** the “Update procedure” for this platform. Prefer a short section in the platform’s Layer 5 doc, or a runbook linked from it. Include: prerequisites, order of steps, rollback, and who executes.
- [ ] **Catalog:** If “Update procedure” is the main entry point, ensure INTEGRATION_PLATFORMS.md “Detailed doc (Layer 5)” or the category note points to it.

#### 3.3 Execute and verify

- [ ] **Execute** per the procedure (staging first if possible).
- [ ] **Update** SDK/version refs in code and in Layer 5 (e.g. “Tested with SDK X.Y”).
- [ ] **Verify** integration behavior (e.g. webhooks, auth, one success path).
- [ ] **Commit** doc and code changes; never commit secrets.

**Output:** Procedure is documented, executed, and verified; docs reflect current versions and config. SYSTEM_MAP and catalog stay accurate.

---

### Step 4: Add or Change “Information” (Documentation Only)

Use when improving docs without changing live settings or code: e.g. adding env var names that were missing, clarifying update steps, or adding links.

#### 4.1 Choose the right place

- [ ] **Catalog (INTEGRATION_PLATFORMS.md):** Short purpose, status, category, “where settings live,” and link to Layer 5. No long how-tos or env lists here.
- [ ] **Layer 5:** Env var names, API/webhook contracts, upgrade/rotation steps, and links to provider docs. Long-form “how to configure X” belongs here.
- [ ] **Runbooks / ops:** If the project keeps runbooks elsewhere, link from Layer 5 or the catalog; avoid duplicating whole procedures in three places.

#### 4.2 Add or edit content

- [ ] **New env var or setting:** Add to Layer 5 first; if it changes “what this platform uses,” add a one-line note in the catalog “Settings / env reference” (e.g. “`OPENAI_*`, `OPENAI_ORG_ID`”).
- [ ] **New update procedure:** Add under “Update procedure” in the platform’s Layer 5 section (or in the linked runbook); ensure the catalog or category note points to it.
- [ ] **New link (e.g. status page, status doc):** Add in Layer 5 or in the platform’s subsection of the catalog.

#### 4.3 Keep cross-references accurate

- [ ] **SYSTEM_MAP:** If the integration’s code path or config location changed, update SYSTEM_MAP and any “See `docs/05-integrations/…`” links.
- [ ] **Behavior/Canon:** If the integration affects product behavior or invariants, ensure behavior and canon docs reference the right platform and, if needed, link to Layer 5.

**Output:** Information lives in the correct layer; catalog and Layer 5 are consistent and discoverable.

---

### Step 5: Retire or Deprecate a Platform

Use when removing an integration or marking it as deprecated (e.g. moving from SendGrid to SES).

#### 5.1 Plan and approve

- [ ] **Confirm** retirement/deprecation is approved (feature request, migration plan, or product decision).
- [ ] **Identify** code paths, env vars, and docs that reference the platform (SYSTEM_MAP, API_MATRIX, Layer 5, INTEGRATION_PLATFORMS).

#### 5.2 Update catalog and Layer 5

- [ ] **Catalog:** Change status to **Deprecated** or **Retired**; add one line (e.g. “Retired YYYY-MM-DD; replaced by X”). Do not delete the row if it helps interpret history or migrations.
- [ ] **Layer 5:** Mark the provider doc as deprecated/archived and point to the replacement (if any). Keep env names for historical/debug use if useful; strip or redact any example values that could be mistaken for live secrets.
- [ ] **SYSTEM_MAP and wiring docs:** Remove or update code path references and “See …” links so they no longer point at the retired platform as current.

#### 5.3 Remove from runtime

- [ ] **Remove** env vars and secrets from all environments only after code and traffic are fully migrated. Document removal in the platform’s Layer 5 section or runbook.
- [ ] **Do not** delete migration-related docs that explain “we used to use X”; archive or clearly label them as historical.

**Output:** Platform is marked deprecated/retired in catalog and Layer 5; code and env no longer depend on it; history is preserved where useful.

---

## Integration with Other Processes

- **INTEGRATION_PLATFORMS.md:** This SOP governs how that doc is updated (add/change/retire platforms, settings references, update procedures). The catalog is the single entry point; Layer 5 holds depth.
- **Layer 5 (`docs/05-integrations/`):** All “how to configure / rotate / upgrade” detail lives here or in linked runbooks. This SOP defines when to create or update those docs.
- **SYSTEM_MAP.md:** When an integration touches code paths or config files, SYSTEM_MAP must be updated as part of adding or retiring a platform (Step 1.4, Step 5.2).
- **Feature requests / Migration:** New integrations that affect product behavior or migration plans should be tracked in feature requests; this SOP covers *documentation and operations* of integration platforms once the decision is made.
- **Canon / Compliance:** If an integration is mandated by canon (e.g. “all email must go via X”), that rule lives in Layer 2; this SOP and the catalog describe *which* platform and *where* its settings are documented.

---

## Notes

- **Secrets:** Never store API keys, passwords, or tokens in the repo or in architecture/docs. Document *names* and *locations* only (e.g. “set in Render env”, “stored in AWS Secrets Manager”).
- **Ownership:** Per platform, designate who may change settings and who may change docs (e.g. “Backend team for Plaid/Ingo; Platform for Render/AWS”). Record in Layer 5 or in the catalog category note if the project formalizes it.
- **Versioning:** When adding “Last Updated” or “SDK version” in Layer 5, use a single place per platform so it’s clear what’s current.
- **Discovery:** New engineers and AI agents should find all integration platforms via `docs/03-architecture/INTEGRATION_PLATFORMS.md` and then drill into Layer 5 for details. Keep the catalog table and category list complete.

---

**Last Updated:** 2026-01-27
