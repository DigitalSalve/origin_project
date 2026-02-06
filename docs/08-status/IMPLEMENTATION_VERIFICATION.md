# Implementation Verification Questions

This document tracks open questions about implementation status that need verification. These questions arose during documentation verification and require code review to answer definitively.

**Procedure:** When and how to add questions, verify, resolve, and tie gaps to feature-request docs are defined in [Implementation Verification SOP](../06-operations/coordination/IMPLEMENTATION_VERIFICATION_SOP.md). Use that SOP as the authority.

**Last Updated:** 2026-01-28  
**Verification Session:** Development status audit

---

## Open Questions

| Question | Source | Status | Assigned | Notes |
|----------|--------|--------|----------|-------|
| Member-app: candidates, deals, home — still stub/mock? | SCOPE_OF_WORK §2 (no mock in user paths); code review | Resolved 2026-01-28 | - | **Resolved:** ApiService now has getCandidates, getCandidate, getDeals, getDeal, redeemDeal. Backend added GET /api/candidates/:id and GET /api/deals/:id. app_candidates_screen, app_candidate_detail_screen, app_deal_detail_screen, and app_home_screen (news, candidates, deals sections) wired to API. Stubs removed. |
| OpenAPI-generated Dart client integrated? | FLUTTER_API_CLIENT_GENERATION; SCOPE_OF_WORK §2 (backend contract) | Open | - | Backend serves `/openapi.json`. Flutter uses hand-written `ApiService` (Dio) only; no generated client. Gap: generate Dart client from OpenAPI and integrate (or explicitly defer and document). |
| RSVP, redeem, sweepstakes winner-accept, profile edit — backend wired and called from Flutter? | Migration Executive Overview (stubbed features); FLUTTER_EVENT_DEAL_SCREENS | Open | - | Event detail uses getEvent() but RSVP button stubbed. Deal redeem, sweepstakes winner-accept, profile edit/delete stubbed in Flutter. Verify backend endpoints exist and wire Flutter to them. |

---

## Resolved Questions

| Question | Source | Resolved | Resolution |
|----------|--------|----------|-------------|
| Member-app: candidates, deals, home — still stub/mock? | SCOPE_OF_WORK §2 | 2026-01-28 | ApiService: getCandidates, getCandidate, getDeals, getDeal, redeemDeal. Backend: GET /api/candidates/:id, GET /api/deals/:id. Screens wired; stubs removed. |

---

## How to Use

1. **When a question is identified** - Add to "Open Questions" with source and context
2. **When investigating** - Update "Assigned" and add notes
3. **When resolved** - Move to "Resolved Questions" with resolution details
4. **When verified** - Update status to ✅ Verified

---

## Verification Process

To verify implementation status:

1. **Code Review** - Check actual implementation in codebase
2. **Migration Review** - Check database migrations for schema changes
3. **Test Review** - Check for tests covering the feature
4. **Documentation Update** - Update relevant docs once verified
5. **Mark Resolved** - Move to resolved section

---

## Common Verification Areas

- **Authentication flows** - Verify JWT, OAuth, session management
- **User roles and permissions** - Verify role-based access control
- **API endpoints** - Verify endpoints match documentation
- **Database schema** - Verify migrations match schema docs
- **UI components** - Verify screens match behavior docs
- **External integrations** - Verify the state voter lookup, payment processors, notifications

---

**Note:** This document helps track gaps between documentation and implementation. Keep it updated as verification work is done.
