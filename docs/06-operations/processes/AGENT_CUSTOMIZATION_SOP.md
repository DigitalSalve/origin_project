# Agent Customization for New Projects — SOP

**Purpose:** Standard Operating Procedure for customizing generic agent definitions from the origin_project template with project-specific context, making them more effective for delegation.

**Status:** Active  
**Applies To:** New project adoption, agent configuration  
**Created:** 2026-02-05

---

## Overview

When adopting infrastructure from origin_project, you get generic agent definitions that work but require agents to rediscover context every time. This SOP shows how to customize agents with:

1. **Project-specific paths** — Actual directory structure
2. **Domain vocabulary** — What the project does, key entities
3. **API and integration details** — Concrete endpoints, auth flows
4. **Technology stack specifics** — Exact frameworks, versions, conventions

**Goal:** Transform generic agents into project-aware specialists.

---

## Prerequisites

- Infrastructure adopted from origin_project
- Agent definitions exist in `.cursor/agents/`
- Documentation complete (Layers 2-4)
- Max Mode enabled for Task tool

---

## Process

### Step 1: Audit Current Project Structure

Document actual paths, tech stack, domain concepts:

```bash
# Map directories
ls -d backend/ marketing/ lib/ webapp/ 2>/dev/null

# Backend dependencies
cat backend/package.json | jq '.dependencies'

# Mobile dependencies  
cat pubspec.yaml | grep -A 10 "dependencies:"
```

Review:
- `docs/02-canon/PROJECT_SCOPE.md` for feature areas
- `docs/03-architecture/SYSTEM_MAP.md` for service layout
- `docs/04-behavior/API_MATRIX.md` for endpoints

---

### Step 2: Customize Backend Agent

**File:** `.cursor/agents/backend.md`

Replace generic sections with concrete details:

**Before:**
```markdown
- **Tech:** Typically Hono/Express. Check project's backend/ for stack.
```

**After:**
```markdown
- **Tech:** Hono + Drizzle + PostgreSQL on Render
- **Paths:**
  - `backend/src/routes/` — Endpoints by domain (auth/, events/, polls/)
  - `backend/src/db/` — Schema and migrations
  - `backend/src/services/` — Business logic
- **Domain:** [e.g., "Civic engagement platform for events, polls, donations"]
- **Key entities:** [e.g., "users, events, polls, candidates"]
```

Add API structure section:

```markdown
## API Structure

**Base URL:** http://localhost:3000/api (dev), https://api.example.com (prod)

**Key endpoints:** (Full list: docs/04-behavior/API_MATRIX.md)
- Authentication: POST /auth/login, POST /auth/verify
- Events: GET/POST /events, POST /events/:id/rsvp
- Polls: GET /polls, POST /polls/:id/vote

**OpenAPI spec:** backend/src/openapi.json

## Integrations

**External services:**
- SendGrid: Transactional email
- Stripe: Payment processing
- AWS S3: File storage
```

---

### Step 3: Customize Frontend Agents

**Web Agent (`.cursor/agents/web.md`):**

```markdown
## Role specifics

- **Tech:** Next.js 14 + React + TypeScript + Tailwind
- **Paths:**
  - `marketing/app/` — Next.js pages
  - `marketing/components/` — Reusable components
  - `marketing/lib/api.ts` — API client
- **Domain:** Public marketing site for [project]
- **Deployment:** Static site on Render → example.com
```

**Mobile Agent (`.cursor/agents/mobile.md`):**

```markdown
## Role specifics

- **Tech:** Flutter 3.x + Dart + Riverpod
- **Paths:**
  - `lib/screens/` — Screen widgets
  - `lib/services/api/` — API client (Dio)
  - `lib/providers/` — Riverpod state
  - `lib/router/` — GoRouter navigation
- **Platforms:** iOS, Android, Web
- **Domain:** Cross-platform member app
```

---

### Step 4: Customize Architect Agent

**File:** `.cursor/agents/architect.md`

Add project context after session start:

```markdown
## Project Context

**Project:** [Name] — [Description]
**Domain:** [What project does]
**Tech stack:**
- Backend: Hono + PostgreSQL
- Frontend: Next.js marketing + Flutter app
- Deployment: Render

**Key workflows:** [e.g., "Event registration, Poll voting, Donations"]

**Documentation:** Read before delegating:
- docs/02-canon/PROJECT_SCOPE.md
- docs/03-architecture/SYSTEM_MAP.md
- docs/04-behavior/API_MATRIX.md
```

Add delegation guidance:

```markdown
## Delegation by Domain

| Domain Area | Subagent | Examples |
|-------------|----------|----------|
| API, database | `/backend` | Add endpoint, migration |
| Marketing site | `/web` | Update homepage, SEO |
| Member app UI | `/webapp` | Dashboard, auth screens |
| Mobile app | `/mobile` | Flutter screens, navigation |
```

---

### Step 5: Test Customized Agents

Validate with Max Mode enabled:

- [ ] Architect session start works
- [ ] Backend delegation uses project paths
- [ ] Agents reference domain vocabulary
- [ ] No "typically" or "check project" language

---

### Step 6: Document Customizations

Create customization log:

```bash
cat > .cursor/agents/CUSTOMIZATION_LOG.md << 'EOF'
# Agent Customization Log

**Date:** 2026-02-05
**Project:** [Name]
**Base:** Infrastructure from ~/Github/origin_project

## Customizations

- **backend.md:** Added Hono stack, actual paths, domain entities
- **web.md:** Added Next.js structure, marketing domain
- **mobile.md:** Added Flutter structure, 30+ screens
- **architect.md:** Added project context, delegation routing

## Maintenance

When pulling blank updates:
1. Merge generic improvements
2. Preserve project-specific sections
3. Re-test delegation
EOF
```

---

## Customization Template

```markdown
## Role specifics

- **Tech:** [Actual frameworks]
- **Paths:** [Actual directories]
- **Domain:** [One-line description]
- **Key entities:** [Main models/features]

## API/Integration Details

[Project-specific structure]

## External Services

- [Service]: [Purpose]
```

---

## Maintenance

**When infrastructure updates:**

1. Pull updates from origin_project
2. Compare agent changes
3. Merge generic improvements
4. Preserve project customizations
5. Re-test

---

## Related Documentation

- [Infrastructure Template Adoption SOP](INFRASTRUCTURE_TEMPLATE_ADOPTION_SOP.md)
- [Custom Agent Creation SOP](CUSTOM_AGENT_CREATION_SOP.md)
- [Agent Testing and Validation SOP](AGENT_TESTING_VALIDATION_SOP.md)

---

**Last Updated:** 2026-02-05
