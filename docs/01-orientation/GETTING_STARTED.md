# Getting Started

This repository uses a **layered documentation structure** as a base for future projects. **We keep the AI instructions and general structure;** we remove only product-specific names and how a particular product is built. You **fill in new project data** in the designated layers when you apply this base to a new project.

**Project name and identity:** This project’s name, slug, repository role, and what is infrastructure vs project-specific are defined in [Project Identity](PROJECT_IDENTITY.md). Use that document when referring to this project by name or when reusing this structure elsewhere.

**Taking this infrastructure to a new project:** To reuse this structure without wiping the source, use [Bootstrap New Project](BOOTSTRAP_NEW_PROJECT.md): copy-this-repo or template-repo workflows, plus an explicit path list for scripting or manual copy.

---

## What We Keep (AI Instructions and General Structure)

- **Project documentation hierarchy** — 8 layers (Orientation, Canon, Architecture, Behavior, Integrations, Operations, Guides, Status). See [Project Documentation Hierarchy](PROJECT_DOCUMENTATION_HIERARCHY.md) for what it is, why it exists, and how it works. The layers and their intent are fixed.
- **Agent loading** — Defined in `docs/README.md`; AI.md points there. Load only layers needed for your task; use Quick Reference. When onboarding, read layers 1–4 in order.
- **Feature request discipline** — No implementation without a feature request. Create Project item with `coordination:feature-request`; see `docs/06-operations/processes/DOCUMENTATION_PROCESS.md`.
- **Documentation Process Enforcement** — Before marking any feature complete, you must update subsystem docs, System Map, migrations/env notes, UI impacts, and create a feature note. See `docs/README.md` (Process and behavior reference) and `docs/06-operations/processes/DOCUMENTATION_PROCESS.md`.
- **Coordination** — Lives in GitHub Project (directives, feature requests, reports, check-outs). See [Coordination Model](COORDINATION_MODEL.md). If not yet migrated, fallback: Active Work in `docs/08-status/`.
- **Screen examination SOP** — Process for examining screens; see `docs/06-operations/examination/SCREEN_EXAMINATION_SOP.md`.
- **General doc structure** — Standard doc names and purposes (e.g. SYSTEM_CANON, SYSTEM_MAP, AUTHENTICATION_FLOWS, API_MATRIX, INTEGRATION_REQUIREMENTS, WEBHOOK_CONTRACTS) are defined in `docs/README.md` and `docs/CHATGPT_PROJECT_DOCUMENTATION_LIST.md`. You create those files and **fill them with your project’s data**.

---

## What You Fill In (New Project Data)

When applying this base to a new project:

1. **Layer 2 (Canon)** — Add project-specific system canon, audit scope, consent/audit docs. See `docs/README.md` Layer 2 for the standard doc names and purposes.
2. **Layer 3 (Architecture)** — Create `docs/03-architecture/` and add SYSTEM_MAP.md, database/entity docs, state machines, boundaries, etc. Fill with your project’s architecture.
3. **Layer 4 (Behavior)** — Create `docs/04-behavior/` and add AUTHENTICATION_FLOWS.md, API_MATRIX.md, UI_INTEGRATION_POINTS.md, DESIGN_SYSTEM.md, etc. Fill with your project’s behavior and UI.
4. **Layer 5 (Integrations)** — Create `docs/05-integrations/` and add INTEGRATION_REQUIREMENTS.md, provider-specific docs, WEBHOOK_CONTRACTS.md. Fill with your project’s external integrations.
5. **Layer 1 (Orientation)** — Create or update [Project Identity](PROJECT_IDENTITY.md) with the new project's name, slug, repository role, and infrastructure vs project-specific boundary. Update this file and/or add project-specific setup (clone, install, run, URLs). Document API server path, frontend path, database path in canon or orientation so key files and routing are documented (see `docs/README.md` and canon/orientation).
6. **Layer 6 (Operations)** — Add deployment, error-handling, archival, or legal/content docs as your project needs them. DOCUMENTATION_PROCESS and SCREEN_EXAMINATION_SOP are already part of the base.

The AI instructions in `AI.md` and `docs/README.md` (how to behave, hard rules, feature request process, documentation checklist, coordination, when to stop and ask) apply unchanged. The **structure** (layers and standard doc names) stays; the **content** is project-specific.

---

## Quick Links

### Documentation
- **[Project Identity](PROJECT_IDENTITY.md)** — This project’s name, slug, role, and infrastructure vs project-specific
- **[Bootstrap New Project](BOOTSTRAP_NEW_PROJECT.md)** — Take this infrastructure to a new project without wiping; workflows and path list
- **[Documentation Index](../README.md)** — Hub for the project documentation hierarchy; links to all layers and standard doc names
- **[AI Guide](../../AI.md)** — Entry point and behavior rules
- **[Guidelines](../02-canon/GUIDELINES.md)** — AI behavior and documentation expectations
- **[Documentation Process](../06-operations/processes/DOCUMENTATION_PROCESS.md)** — Feature request lifecycle and doc checklist
- **[Current Status](../08-status/CURRENT_STATUS.md)** — Latest development updates
- **[Documentation List for AI Projects](../CHATGPT_PROJECT_DOCUMENTATION_LIST.md)** — Full structure and order for AI context

### Coordination
- **[Coordination Model](COORDINATION_MODEL.md)** — What lives in GitHub Project vs docs; full flow and reasoning
- **[GitHub Coordination Setup Guide](../07-guides/GITHUB_COORDINATION_SETUP_GUIDE.md)** — Set up Project, labels, views
- **[Migration Plan](../08-status/GITHUB_COORDINATION_MIGRATION_PLAN.md)** — For projects with docs-based coordination

---

## How to Use This Structure

1. **Read in order** — Follow the agent loading in `docs/README.md` (AI.md → this file → layers as needed; use Quick Reference).
2. **Respect canon** — Layer 2 is truth. Do not violate it.
3. **Create feature requests** — Before implementing features, create a Project item with label `coordination:feature-request`; spec in item body.
4. **Check out before editing** — Create Project item (Active Work view) or use `ACTIVE_WORK.md` if not yet migrated. See [Coordination Model](COORDINATION_MODEL.md).
5. **Fill layers with project data** — When starting a new project, create the layer folders and standard docs listed in `docs/README.md` and fill them with that project’s content.

---

## Installation & Setup (Project-Specific)

**This base does not assume a specific stack.** When you apply it to a project, document that project’s setup here (or in a linked doc).

Example placeholders:

```text
# Clone the repository
git clone <repository-url>

# Navigate to project
cd <project-directory>

# Install dependencies
npm install   # or yarn, pnpm, etc.

# Start development server
npm run dev  # or project-specific command

# Open app (e.g. http://localhost:5173)
```

Replace with your project’s values and add any env vars, migrations, or prerequisites. Document API server path, frontend path, and database/migrations path in canon or here so “Key files and routing” in `AI.md` is complete for that project.

---

## Summary

You have:
1. **AI instructions** — Unchanged (behavior rules, feature requests, documentation checklist, coordination, when to stop and ask).
2. **General structure** — Unchanged (8 layers, standard doc names and purposes).
3. **A base to fill** — Create the layer docs in `docs/README.md` and `CHATGPT_PROJECT_DOCUMENTATION_LIST.md` and fill them with your project’s data.

**Start from:** [Project Documentation Hierarchy](PROJECT_DOCUMENTATION_HIERARCHY.md) (explanation), [Documentation Index](../README.md) (navigation), and [AI Guide](../../AI.md) (agent entry).
