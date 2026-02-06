# Origin Project Template Audit

**Date:** 2026-01-28  
**Purpose:** Identify orphaned content, misleading instructions, and flow non-compliance in the origin_project infrastructure template.

---

## Executive Summary

origin_project contains **project-specific contamination** (sourceapp references, broken links to non-existent files) and **flow inconsistencies** (REPORT_LOG vs Project-based reports; docs-based feature-requests vs Project). Several SOPs and docs still reference deprecated patterns. Fixes required for template compliance.

---

## 1. Orphaned Content

| Item | Location | Issue |
|------|----------|-------|
| **Duplicate docs at docs/ root** | `docs/BOOTSTRAP_NEW_PROJECT.md`, `docs/CORE_INFRASTRUCTURE_INVENTORY.md`, `docs/GETTING_STARTED.md` | docs/README links to `01-orientation/` versions; root copies are orphans |
| **REPORT_LOG.md** | `08-status/reports/REPORT_LOG.md` | reports/README is Project-based; REPORT_LOG is legacy; file contains sourceapp-specific entries |

---

## 2. Broken Links (docs/README)

| Link | Target | Exists in origin_project? |
|------|--------|--------------------------|
| Layer 4: AUTHENTICATION_FLOWS, COOKIES_AND_SESSION, API_MATRIX, UI_INTEGRATION_POINTS, DESIGN_SYSTEM, SUBSCRIPTION_TIERS, INTENDED_USER_FLOWS | `04-behavior/*.md` | No — origin_project has only `04-behavior/README.md` |
| Layer 3: INTEGRATION_PLATFORMS | `03-architecture/INTEGRATION_PLATFORMS.md` | No |
| Layer 8: 2026-01-27/2026-01-28 Screen Examination Reports | `08-status/screen-examination/*.md` | No — origin_project has only README |
| Layer 8: 2026-01-27 Backend Examination Report | `08-status/backend-examination/*.md` | No |
| Quick Reference: "Understand web implementation" | sourceapp-specific report + SOURCEAPP_FRONTEND_BACKEND_ARCHITECTURE_SOP | No — origin_project has no SOURCEAPP SOP |

---

## 3. Misleading Instructions (Flow Non-Compliance)

| Doc | Issue |
|-----|-------|
| **docs/README Layer 8** | "Reports Log" and "REPORT_LOG" — reports/README is Project-based (`coordination:report`) |
| **docs/README Quick Reference** | "reports in 08-status/reports/ with log in REPORT_LOG" — contradicts Project flow |
| **CORE_INFRASTRUCTURE_INVENTORY** | agent-coordination: "AGENT_DIRECTIVES + AGENT_COORDINATION_LOG" — coordination is in Project; those files don't exist in origin_project |
| **CORE_INFRASTRUCTURE_INVENTORY** | reports: "REPORT_LOG format" — should note Project-based |
| **REPORT_CREATION_AND_DELIVERY_SOP** | Heavy REPORT_LOG usage; no Project-based flow as primary |
| **agent-coordination/README** | "Project items with label `agent-coordination`" — Process table says `coordination:q-a` for Q&A |
| **GAP_IDENTIFICATION_SOP** | References `docs/08-status/feature-requests/` — should add Project as primary |
| **SCOPE_OF_WORK_SOP** | References `docs/08-status/feature-requests/` — should add Project as primary |
| **USER_FLOW_IDENTIFICATION_SOP** | "Log in REPORT_LOG" — Project flow for reports |
| **user-flows/README** | "Add to REPORT_LOG" — Project flow |
| **feature-requests/README** | No deprecation note; instructs docs-based flow |

---

## 4. Project-Specific Contamination

| Doc | Issue |
|-----|-------|
| **CURRENT_STATUS.md** | Full sourceapp deploy status, Render services, feature tables |
| **FEATURE_REQUEST_TODO.md** | Full sourceapp FR list with implementation details |
| **PROJECT_IDENTITY.md** | "sourceapp" as slug — acceptable as example |
| **REPORT_LOG.md** | sourceapp report entries |
| **ACTIVE_WORK_LOG.md** | sourceapp check-out history |

**Note:** origin_project is a template. Status docs (CURRENT_STATUS, FEATURE_REQUEST_TODO, ACTIVE_WORK_LOG, REPORT_LOG) should be placeholder/empty or explicitly "fill per project." Heavy sourceapp content makes origin_project unsuitable as a clean template.

---

## 5. Recommendations

### Critical (Flow Compliance)
1. Update docs/README Layer 4 to generic "fill per project" structure (no links to non-existent files)
2. Update docs/README Layer 8 to generic structure (no sourceapp-specific report links)
3. Update docs/README: Reports → Project-based; remove or qualify REPORT_LOG references
4. Update REPORT_CREATION_AND_DELIVERY_SOP: Project-based as primary; REPORT_LOG as optional fallback
5. Update agent-coordination/README: `coordination:q-a` for Q&A (not `agent-coordination`)
6. Update CORE_INFRASTRUCTURE_INVENTORY: agent-coordination (Project, no AGENT_DIRECTIVES/LOG); reports (Project-based)

### High (SOP Updates)
7. GAP_IDENTIFICATION_SOP, SCOPE_OF_WORK_SOP: Add Project as primary for feature requests
8. USER_FLOW_IDENTIFICATION_SOP, user-flows/README: Project-based report flow
9. feature-requests/README: Add deprecation note; point to Project

### Medium (Orphans & Sanitization)
10. Remove or consolidate docs/ root duplicates (BOOTSTRAP, CORE_INFRASTRUCTURE, GETTING_STARTED)
11. Sanitize CURRENT_STATUS, FEATURE_REQUEST_TODO, ACTIVE_WORK_LOG, REPORT_LOG to placeholder content
12. Remove Quick Reference "Understand web implementation" (sourceapp-specific) or make generic
