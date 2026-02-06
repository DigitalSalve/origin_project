# Infrastructure: Auth Mode and Push Options

**Purpose:** Defines **core infrastructure** for selectable authentication mode (internal vs Firebase) and for push notifications (Firebase Cloud Messaging). Admin can enable/disable **internal auth** and **Firebase auth** so the project can use backend-owned auth, Firebase Auth, or both (with one primary for member login).

**Use when:** Implementing or configuring auth (internal magic-link + password + OAuth vs Firebase Auth) and mobile push (FCM). Apply per project; see [PROJECT_IDENTITY](../01-orientation/PROJECT_IDENTITY.md) for project slug and naming.

**Related:** [INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) (internal auth flow), [FLUTTER_SERVICE_LAYER_ARCHITECTURE](../08-status/feature-requests/FLUTTER_SERVICE_LAYER_ARCHITECTURE.md) (push service).

---

## 1. Auth mode (summary)

Two auth modes are supported as **core infrastructure**:

| Mode | Description | When enabled |
|------|-------------|--------------|
| **Internal auth** | Backend-owned magic-link, email+password, and OAuth (Google, Facebook, Apple). Sessions issued by the backend; user and session stored in project DB. | Login/register use project API; backend issues session cookie / Bearer token. |
| **Firebase auth** | Firebase Authentication as identity provider. Client signs in with Firebase (email/password, email link, or OAuth); backend verifies Firebase ID token and creates/updates user and session in project DB. | Login/register use Firebase SDK; client sends Firebase ID token to backend; backend verifies token and issues project session. |

**At least one** mode must be enabled. Admin can enable **internal only**, **Firebase only**, or **both** (e.g. internal for admin, Firebase for members — or both for members with UI showing one primary).

---

## 2. Admin settings (auth mode)

Auth mode is controlled by **admin settings**, stored in the database (e.g. `app_settings` or `feature_flags` table) or in environment/config that admin UI can change when permitted.

| Setting key | Type | Description | Default |
|-------------|------|-------------|---------|
| `internal_auth_enabled` | boolean | When true, backend accepts internal auth (magic-link, password, OAuth) and issues sessions. | `true` |
| `firebase_auth_enabled` | boolean | When true, backend accepts Firebase ID tokens (e.g. `POST /api/auth/firebase` with `id_token`), verifies with Firebase Admin SDK, and creates/updates user and session. | `false` |

**Admin UI:** Admin dashboard MUST expose an **Auth / Settings** (or **Authentication**) section where a super-admin can:

- **Enable / disable internal auth** — when disabled, login/register forms that use backend magic-link or password are hidden or show "Internal auth is disabled."
- **Enable / disable Firebase auth** — when enabled, client shows Firebase sign-in (e.g. email/password or "Sign in with Google" via Firebase); when disabled, Firebase sign-in is hidden.

**Backend behavior:**

- If **internal auth enabled:** Existing internal auth routes (e.g. `POST /api/auth/login`, `POST /api/auth/register`, magic-link, OAuth) work as today. Middleware or route guards MUST check `internal_auth_enabled` and return 503 or 404 with a clear message when disabled.
- If **Firebase auth enabled:** Backend MUST expose a route (e.g. `POST /api/auth/firebase` or `POST /api/auth/firebase/token`) that accepts `id_token` (or `access_token` for OAuth), verifies it with Firebase Admin SDK, then creates or updates the user in project DB and issues the project session (cookie or Bearer token). Same session semantics as internal auth (e.g. `<project_slug>_session`, onboarding_complete per project).

**Client behavior:**

- Client MUST read auth-mode settings (e.g. from `GET /api/settings/public` or embedded in app config) and show only the login/register options that are enabled. When both are enabled, project may define a primary (e.g. "Sign in with email" = internal vs "Sign in with Google" = Firebase).

---

## 3. Push notifications (Firebase Cloud Messaging)

**Push** is part of core infrastructure for **mobile only** (iOS/Android). Web does not use FCM.

| Component | Description |
|-----------|-------------|
| **Provider** | Firebase Cloud Messaging (FCM). |
| **Client** | Flutter `firebase_messaging` package; platform-specific setup (APNs for iOS, FCM for Android). |
| **Backend** | Store FCM tokens per user/device (e.g. `user_push_tokens` or equivalent); send messages via Firebase Admin SDK or FCM HTTP v1 API. Existing notification/member-notifications and ads-push services can target FCM tokens when push is enabled. |

**Admin / user control:**

- **Admin:** May have a global or per-campaign toggle to enable/disable push delivery (e.g. "Send push notifications" in notification or ads settings). Optional.
- **User:** User preferences (e.g. "Push notifications" on/off in profile or settings) MUST be respected; backend and client must not send or register for push when user has opted out.

**When to implement:** Per [FLUTTER_SERVICE_LAYER_ARCHITECTURE](../08-status/feature-requests/FLUTTER_SERVICE_LAYER_ARCHITECTURE.md), push notification service (FCM) is a planned service-layer item. Implement when member app and admin notification flows are in scope.

---

## 4. Implementation checklist (auth mode)

- [ ] **Backend:** Add `internal_auth_enabled` and `firebase_auth_enabled` to app settings (DB or config). Expose via admin API and, if needed, a public or unauthenticated endpoint for client (e.g. which login options to show).
- [ ] **Backend:** Gate internal auth routes with `internal_auth_enabled`; when false, return 503 or 404 with message.
- [ ] **Backend:** When `firebase_auth_enabled` is true, implement `POST /api/auth/firebase` (or equivalent) that accepts Firebase ID token, verifies with Firebase Admin SDK, creates/updates user, issues project session.
- [ ] **Admin UI:** Add Auth / Authentication settings screen with toggles: "Internal auth (magic-link, password, OAuth)" and "Firebase auth" (enable/disable).
- [ ] **Client:** Fetch auth-mode settings at app start or login screen load; show only enabled login/register options.

---

## 5. Implementation checklist (push)

- [ ] **Backend:** Ensure FCM token storage (e.g. `user_push_tokens`) and send path exist; wire to notification and ads-push services when FCM is used.
- [ ] **Client:** Implement push notification service (FCM) per [FLUTTER_SERVICE_LAYER_ARCHITECTURE](../08-status/feature-requests/FLUTTER_SERVICE_LAYER_ARCHITECTURE.md); register token with backend; respect user push preference.
- [ ] **Admin:** Optional toggle to enable/disable push delivery globally or per campaign.

---

## 6. Environment / config (Firebase)

When Firebase auth or FCM is enabled:

| Variable / config | Purpose |
|-------------------|---------|
| `FIREBASE_PROJECT_ID` | Firebase project ID. |
| `FIREBASE_CLIENT_EMAIL` / `FIREBASE_PRIVATE_KEY` (or service account JSON) | Firebase Admin SDK credentials for backend token verification and FCM send. |
| Client: Firebase config (e.g. `google-services.json` / `GoogleService-Info.plist`) | Flutter app uses these for Firebase Auth and FCM on device. |

Store secrets in environment or secret manager; never in client code.

---

## 7. Revision history

| Date | Change |
|------|--------|
| 2026-01-28 | Initial: auth mode (internal vs Firebase), admin toggles, FCM as core infra for push. |
