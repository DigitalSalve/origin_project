# Infrastructure: Authentication — Core Flow

**Purpose:** This document defines the **recurring authentication method** used across projects so you don’t have to figure it out again for each one. It describes passwordless magic-link + backup-code auth end-to-end (web, API, database, workers, mobile) as **core infrastructure**.

**Use when:** You need the standard flow: passwordless magic link + 6-digit backup code, role-based entry, cookie-based web sessions, device-bound mobile sessions, and trusted-device semantics. Apply this pattern and adapt route prefixes, cookie names, and roles per project (see PROJECT_IDENTITY for project slug).

---

## 1. Authentication model (summary)

- **Method:** Passwordless — magic link + 6-digit backup code.
- **Link expiry:** 15 minutes.
- **Roles:** Define per project (e.g. homeowner, contractor, admin — or whatever the project needs).
- **Web:** Cookie-based session; optional device/fingerprint; “remember this device” → trusted device.
- **Mobile:** Device-bound sessions; **required** `device_id` and `client_type: 'mobile'`; Bearer token in `Authorization`; token in secure storage (e.g. Keychain/Keystore); no cookies. Backend owns “trusted device” via device APIs.

---

## 2. Web

### 2.1 Entry routes

Per-role login entry points. Example pattern:

- `/<role>/login` — e.g. `/homeowner/login`, `/contractor/login`, `/admin/login`.

Frontend uses these to show the correct login form and to pass role context (if needed) when calling the auth API.

### 2.2 Flow

1. User submits **email** (and optionally role) on the login page.
2. Frontend calls **request magic link** (e.g. `POST /api/auth/magic-link`) with email, optional `device_id`, optional `client_type: 'web'`, optional `browser_fingerprint`.
3. Backend creates a magic link and (if configured) sends email with link and 6-digit code; stores hashed token and code, expiry, optional device/fingerprint scope.
4. User either:
   - Clicks link → backend verifies token (e.g. `GET /api/auth/verify-link/:token` or `POST /api/auth/verify-link`) and establishes session; or
   - Enters 6-digit code → frontend calls `POST /api/auth/verify-code` with email, code, optional `client_type: 'web'`.
5. On success, backend sets **session cookie** and optional trusted-device cookie; frontend redirects to post-login destination.

### 2.3 Cookie name and semantics

- **Session cookie:** Use a single project-specific name: `<project_slug>_session` (e.g. `sourceapp_session`). Value is a signed token; verify and parse on each request. Attributes: `HttpOnly`, `Secure` in production, `SameSite` (e.g. `Lax` or `Strict`), and path/domain as required.
- **Trusted device (optional):** If you support “remember this device” on web, use a second cookie: `<project_slug>_trusted_device`. Semantics and lifetime are defined by backend policy.

### 2.4 Optional device / fingerprint (web)

- **device_id** / **client_type:** Optional for web. When sent, they can be stored on the magic link and/or session for audit or binding.
- **browser_fingerprint:** Optional; stored on `magic_links` for binding link to browser. Backend may validate that the same fingerprint is used at verify time.

### 2.5 How the frontend calls the auth API

- **Request magic link:** `POST /api/auth/magic-link` — body: `{ email, role? }`; optional headers or body: `device_id`, `client_type: 'web'`, `browser_fingerprint`.
- **Verify link:** `GET /api/auth/verify-link/:token` (redirect from email) or `POST /api/auth/verify-link` — body/query: token; optional device/fingerprint if used.
- **Verify code:** `POST /api/auth/verify-code` — body: `{ email, code }`; optional `client_type: 'web'`.
- **Registration:** If registration is separate from magic link, e.g. `POST /api/auth/register/:role` with body `{ email, name? }`.
- All responses that create a session set the session cookie via `Set-Cookie`; frontend follows redirects or uses response status/body to transition to authenticated UI.

---

## 3. API

### 3.1 Web auth routes (pattern)

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/api/auth/register/homeowner` | Register homeowner (or equivalent role) |
| POST | `/api/auth/register/contractor` | Register contractor (or equivalent role) |
| POST | `/api/auth/magic-link` | Request magic link (email, optional device/fingerprint) |
| GET | `/api/auth/verify-link/:token` | Verify magic link from email click |
| POST | `/api/auth/verify-link` | Verify magic link (body/query token) |
| POST | `/api/auth/verify-code` | Verify 6-digit code (email + code) |

Session validation and cookie clearing live in the same API layer. Auth feature flags (e.g. `auth_login_enabled`) are checked where login is gated.

### 3.2 Mobile auth and device routes (pattern)

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/api/mobile/auth/register` | Register user from mobile |
| POST | `/api/mobile/auth/magic-link` | Request magic link (email, device_id, client_type: 'mobile') |
| GET | `/api/mobile/auth/verify` | Verify link/code (token or code, device_id, client_type) |
| POST | `/api/mobile/auth/login` | Login with code (email, code, device_id, client_type) |
| GET | `/api/mobile/auth/session` | Validate session / return current session info |
| POST | `/api/mobile/auth/logout` | Logout and revoke session |
| POST | `/api/mobile/devices/register` | Register device before or during auth |
| GET | `/api/mobile/devices` | List user’s devices |
| GET | `/api/mobile/devices/trusted/status` | Get trusted status for current device |
| DELETE | `/api/mobile/devices/:device_id` | Remove a device |
| POST | `/api/mobile/devices/trusted` | Mark device as trusted (“remember this device”) |
| DELETE | `/api/mobile/devices/trusted/:device_id` | Revoke trusted status for a device |

### 3.3 Request/response shapes (conceptual)

- **Magic link request:**  
  - Body: `{ email }`; mobile: `{ email, device_id, client_type: 'mobile' }`.  
  - Response: 200/202 with message that email was sent; or 4xx with error message.
- **Verify link:**  
  - Query or body: `token`; mobile: plus `device_id`, `client_type`.  
  - Response: 200 + session (cookie for web; body token for mobile); or 4xx.
- **Verify code:**  
  - Body: `{ email, code }`; mobile: plus `device_id`, `client_type`.  
  - Response: 200 + session (cookie for web; body token for mobile); or 4xx.
- **Session:**  
  - Web: session established via `Set-Cookie`.  
  - Mobile: session token in response body; client stores it and sends `Authorization: Bearer <token>` on subsequent requests.

### 3.4 Headers

- **Authorization:** `Bearer <session_token>` — used by mobile clients on all authenticated requests. Not used for web (cookie only).
- **X-Device-ID:** Device identifier; **required** for mobile auth and device APIs; optional for web.
- **X-Client-Type:** `web` | `mobile` — required for mobile; optional for web.

Session validation middleware resolves the user from the cookie (web) or from the Bearer token (mobile), then attaches user/role to the request.

### 3.5 Session validation and cookie handling

- **Web:** On each request, read the session cookie, verify signature (using `SESSION_SIGNING_KEY`), resolve user and optional role, attach to request. On logout or explicit revoke, clear the session cookie and revoke the session server-side.
- **Mobile:** On each request, read `Authorization: Bearer <token>`, verify token, resolve user/device, attach to request. Logout/revoke invalidates the token and clears client-side storage.

### 3.6 Environment variables (API)

| Variable | Purpose |
|----------|---------|
| `SESSION_SIGNING_KEY` | Secret used to sign and verify session tokens (web cookie and mobile Bearer). **Required** for the API that issues and validates sessions. |
| `DATABASE_URL` | Postgres connection for users, magic_links, auth_sessions, devices, trusted_devices. |
| Email provider (e.g. SendGrid) | Sending magic-link emails (API key, from address, templates). |
| CORS config | Allowed origins for web and (if applicable) mobile. |
| App identifiers (bundle ID / package name) | For deep links and optional app-specific validation. |

---

## 4. Database

### 4.1 Migration order (auth-related)

Apply in this order when implementing the pattern:

1. **Base auth** — `users`, `magic_links`, `auth_sessions`
2. **Session policies** — `auth_sessions.last_activity_at`, `revoked_reason`
3. **Trusted devices** — `trusted_devices`
4. **Device scope on links** — `magic_links.device_id`, `magic_links.client_type`
5. **Mobile devices** — `devices`, `push_tokens`; `auth_sessions` and `trusted_devices` gain `device_id`, `client_type`, FKs to `devices`
6. **Fingerprint on links (optional)** — `magic_links.browser_fingerprint`

Exact migration filenames (e.g. `002_auth.sql`, `007_auth_session_policies.sql`) may differ per project; the dependency order above is what matters.

### 4.2 Tables and main columns

- **users** — `id`, `email`, `role`, `name`, `created_at`. Canonical identity.
- **magic_links** — `id`, `user_id`, `token_hash`, `code_hash`, `expires_at`, `consumed_at`, `created_at`; optionally: `device_id`, `client_type`, `browser_fingerprint`. One-time use; 15-minute expiry.
- **auth_sessions** — `id`, `user_id`, `token_hash`, `created_at`, `revoked_at`; optionally: `actor_role`, `actor_email`, `last_activity_at`, `revoked_reason`, `device_id`, `client_type`, FK to `devices`. Signed token in cookie or Bearer corresponds to `token_hash`.
- **trusted_devices** — `id`, `user_id`, `token_hash`, `device_label`, `created_at`, `last_used_at`, `revoked_at`; optionally: `device_id`, `client_type`, FK to `devices`. Backend source of truth for “trust this device.”
- **devices** — `id`, `device_id` (external id from client), `user_id`, `platform`, `device_info`, `status`, etc. Registered devices for mobile.
- **push_tokens** — Optional; link to `devices` or `user_id` for push notifications.

Primary keys, indexes, and FKs follow from these; implement unique constraints and indexes per your query patterns (e.g. lookup by token_hash, by device_id, by user_id).

---

## 5. Workers

- **Workers do not participate in auth.** They do not issue or validate sessions, and they do not have access to `SESSION_SIGNING_KEY`.
- **Workers** perform background jobs (e.g. sending emails, processing events). If they need to act “as” a user, they receive user identifiers from the API or from queues, not from session tokens. No session middleware.
- When implementing this pattern, keep worker env minimal: e.g. `DATABASE_URL`, queue/email credentials. Omit `SESSION_SIGNING_KEY`.

---

## 6. iOS / Mobile (client)

### 6.1 Flow

1. **Request magic link:**  
   `POST /api/mobile/auth/magic-link` with `{ email, device_id, client_type: 'mobile' }`. Optionally call `POST /api/mobile/devices/register` first with device info.
2. **Verify:**  
   - Link: open deep link or use `GET /api/mobile/auth/verify?token=...&device_id=...&client_type=mobile`.  
   - Code: `POST /api/mobile/auth/login` with `{ email, code, device_id, client_type: 'mobile' }`.
3. **Session:**  
   Response includes session token; store in **secure storage** (Keychain on iOS, Keystore on Android). Send `Authorization: Bearer <token>` and `X-Device-ID`, `X-Client-Type: mobile` on every authenticated request.
4. **Device registration:**  
   Ensure device is registered (`POST /api/mobile/devices/register`) when using the mobile flow; required before or during first login so backend can bind session to device.
5. **Trusted device:**  
   Backend owns trust. App calls `GET /api/mobile/devices/trusted/status`. If the device is **not** trusted, app clears local session on startup and forces re-login. If trusted, app may keep session until expiry or logout.

### 6.2 Headers and storage

- **Headers:** `Authorization: Bearer <token>`, `X-Device-ID: <device_id>`, `X-Client-Type: mobile`.
- **Storage:** Session token in platform secure storage only; no cookies. Device ID can be generated once per install and persisted (e.g. in secure storage or private prefs).

### 6.3 Trusted-device behavior

- **“Trust this device”** is decided and stored on the backend (`trusted_devices`, `devices`). The app asks `GET /api/mobile/devices/trusted/status` and, if not trusted, **clears the local session on app start** so the user must log in again. This avoids long-lived sessions on untrusted devices.

---

## 7. Deployment

- **Web (static):** Served from CDN or static host; no session logic.
- **API:** Process that serves the web API and (if applicable) static site or reverse-proxied SPA; env includes `SESSION_SIGNING_KEY`, `DATABASE_URL`, email, CORS, app identifiers.
- **Worker:** Separate process; no `SESSION_SIGNING_KEY`; has `DATABASE_URL` and any queue/email credentials.
- **Database:** Postgres; run migrations in the order given in §4.1.

---

## 8. Implementation checklist

Use this when applying the pattern in a new project so you don’t have to re-derive it.

| Area | Checklist |
|------|------------|
| **Database** | Run auth migrations in order (§4.1). Ensure `users`, `magic_links`, `auth_sessions`, `trusted_devices`, `devices` (and optionally `push_tokens`) exist with expected columns and constraints. |
| **Env (API)** | Set `SESSION_SIGNING_KEY`, `DATABASE_URL`, email provider config, CORS, app bundle/package IDs for deep links. |
| **Env (Worker)** | Set `DATABASE_URL` and job-related secrets only; **do not** set `SESSION_SIGNING_KEY`. |
| **Web routes** | Implement `POST /api/auth/magic-link`, `GET /api/auth/verify-link/:token`, `POST /api/auth/verify-link`, `POST /api/auth/verify-code`; add register-by-role if needed. Session cookie name and attributes per §2.3. |
| **Mobile routes** | Implement `/api/mobile/auth/*` and `/api/mobile/devices/*` as in §3.2. Require `device_id` and `client_type: 'mobile'` for mobile auth. |
| **Headers** | Web: session via cookie only. Mobile: `Authorization: Bearer <token>`, `X-Device-ID`, `X-Client-Type: mobile`. |
| **Client (web)** | Call magic-link and verify-code endpoints; follow redirects; rely on `Set-Cookie` for session. |
| **Client (mobile)** | Request link → verify link or code → store token in secure storage → send Bearer + device headers; on startup, call trusted status and clear session if not trusted. |

---

## 9. Per-project adaptation

This pattern is part of core infrastructure. When applying it to a project:

- **Cookie names:** Use `<project_slug>_session` and `<project_slug>_trusted_device`; project slug is in [PROJECT_IDENTITY](../01-orientation/PROJECT_IDENTITY.md).
- **Roles and paths:** Adapt `/api/auth/register/:role` and `/<role>/login` to the project’s roles and entry routes.
- **Route prefix:** Keep `/api/auth/*` and `/api/mobile/auth/*`, `/api/mobile/devices/*` unless the project canon explicitly defines a different layout.

---

**Last updated:** 2026-01-27
