# Screen Examination SOP

**Purpose:** Standard Operating Procedure for AI agents to systematically examine screens and associated functionality to identify broken or stubbed content.

**Status:** Active

**Applies To:** All repositories (web, mobile, and other applications)

---

## Overview

This SOP defines a systematic process for AI agents to:
1. Identify all screens/routes in the application
2. Examine each screen for broken functionality, stubbed content, or incomplete implementations
3. Document findings in a structured format
4. Prioritize issues based on severity and user impact

---

## Prerequisites

Before beginning a screen examination:

1. **Read relevant documentation:**
   - `AI.md` (repository-specific AI behavior rules)
   - `docs/README.md` (documentation navigation)
   - `docs/04-behavior/` (user flows and UI integration points)
   - `docs/03-architecture/` (system architecture and data models)

2. **Understand the application structure:**
   - **Web applications:** React Router routes, Next.js pages, or similar routing configuration
   - **Mobile applications:** Flutter screens, React Native screens, or native navigation
   - **Other frameworks:** Identify the routing/navigation system used

3. **Identify user roles:**
   - List all user roles/types in the application
   - Understand role-based access and permissions

---

## Examination Process

### Step 1: Inventory All Screens

#### For Web Applications

1. **Read routing configuration:**
   - Open routing files (e.g., `App.tsx`, `routes.tsx`, `pages/` directory)
   - Extract all route definitions
   - Group by user role or feature area

2. **Map routes to components:**
   - For each route, identify the component file
   - Note the file path and verify it exists
   - Document the component structure

3. **Create a screen inventory table:**

```markdown
| Route | Component | Role | Status | Notes |
|-------|-----------|------|--------|-------|
| /dashboard | Dashboard | User | ✅ | |
| /settings | Settings | User | ⚠️ | Stub detected |
```

#### For Mobile Applications

1. **Read screen files:**
   - List all files in screen/page directories
   - Identify screen widgets/components
   - Map navigation flows

2. **Map screens to navigation:**
   - Check navigation configuration files
   - Identify navigation flows between screens
   - Document route definitions

3. **Create a screen inventory table:**

```markdown
| Screen File | Screen Name | Role | Status | Notes |
|-------------|-------------|------|--------|-------|
| screens/home_screen.dart | Home Screen | User | ✅ | |
| screens/settings_screen.dart | Settings | User | ⚠️ | Incomplete |
```

---

### Step 2: Examine Each Screen

For each screen identified in Step 1, perform the following checks:

#### 2.1 Component/Screen File Existence

- [ ] **File exists:** Verify the component/screen file exists at the expected path
- [ ] **File is not empty:** File contains actual implementation (not just a placeholder)
- [ ] **File imports are valid:** All imports resolve correctly

**What to look for:**
- Missing files
- Empty files or files with only comments
- Import errors (broken module paths)

#### 2.2 UI Rendering

- [ ] **Component renders:** Component exports correctly and can be rendered
- [ ] **No console errors:** Check browser console (web) or logs (mobile) for errors
- [ ] **Layout is complete:** UI elements are properly structured, not cut off or overlapping

**What to look for:**
- Components that fail to render
- Layout issues (overflow, misalignment)
- Missing UI elements referenced in code

#### 2.3 Stubbed Content Detection

- [ ] **No placeholder text:** Check for placeholder strings like:
  - "TODO", "FIXME", "STUB", "PLACEHOLDER"
  - "Coming soon", "Not implemented", "Under construction"
  - Lorem ipsum or dummy text
- [ ] **No mock data:** Verify data comes from real API calls or state, not hardcoded mocks
- [ ] **No disabled features:** Check for commented-out code or disabled buttons/links

**What to look for:**
```typescript
// Example stubbed content patterns:
<div>TODO: Implement this</div>
<button disabled>Coming soon</button>
const data = mockData; // Should be API call
// FIXME: This is incomplete
```

#### 2.4 Navigation and Links

- [ ] **Internal links work:** All internal navigation links resolve to valid routes
- [ ] **External links are valid:** External URLs are correct and accessible
- [ ] **Back navigation works:** Back buttons/links function correctly
- [ ] **Breadcrumbs are accurate:** If present, breadcrumbs reflect current location

**What to look for:**
- Broken internal links (404s, wrong routes)
- Dead external links
- Navigation loops or missing return paths

#### 2.5 API Integration

- [ ] **API calls exist:** Screen makes necessary API calls to fetch data
- [ ] **API endpoints are correct:** Endpoints match documented API routes
- [ ] **Error handling:** API errors are handled gracefully (not crashing the UI)
- [ ] **Loading states:** Loading indicators appear during API calls
- [ ] **Empty states:** Empty data states are handled (no data, no results)

**What to look for:**
- Missing API calls (data never loads)
- Incorrect API endpoints
- Unhandled API errors (crashes, uncaught exceptions)
- Missing loading/empty states

#### 2.6 Form Functionality (if applicable)

- [ ] **Form validation:** Input validation works (required fields, format checks)
- [ ] **Form submission:** Submit buttons trigger correct actions
- [ ] **Form reset:** Reset/clear functionality works
- [ ] **Form state persistence:** Form data persists appropriately (or clears as expected)

**What to look for:**
- Forms that don't validate
- Submit buttons that do nothing
- Forms that don't clear after submission
- Lost form data on navigation

#### 2.7 User Role Permissions

- [ ] **Access control:** Screen respects user role permissions
- [ ] **Unauthorized access:** Unauthorized users are redirected appropriately
- [ ] **Feature flags:** Feature flags are respected (if applicable)

**What to look for:**
- Screens accessible to wrong roles
- Missing permission checks
- Feature flags not enforced

#### 2.8 State Management

- [ ] **State updates correctly:** UI updates when state changes
- [ ] **State persists:** State persists across navigation (if required)
- [ ] **State resets:** State resets appropriately when leaving screen

**What to look for:**
- UI not updating after state changes
- State leaking between screens
- State not resetting when expected

---

### Step 3: Test User Flows

For each major user flow, test the complete journey:

1. **Authentication Flow:**
   - Login → Dashboard
   - Registration → Email verification → Dashboard
   - Password recovery → Reset → Login

2. **Core Feature Flows:**
   - Identify primary user journeys
   - Test end-to-end flows
   - Verify navigation between screens

**What to look for:**
- Broken flows (can't complete the journey)
- Missing steps in flows
- Incorrect redirects
- Lost context between screens

---

### Step 4: Document Findings

Create a structured findings document:

#### 4.1 Create Findings File

Create a new file: `docs/08-status/screen-examination/{DATE}_SCREEN_EXAMINATION_REPORT.md`

#### 4.2 Document Structure

```markdown
# Screen Examination Report

**Date:** YYYY-MM-DD
**Examiner:** AI Agent
**Repository:** [Repository name]
**Scope:** [Full application / Specific role / Specific feature]

---

## Executive Summary

- Total screens examined: X
- Screens with issues: Y
- Critical issues: Z
- High priority issues: A
- Low priority issues: B

---

## Screen-by-Screen Findings

### [Screen Name] - [Route/Path]

**Status:** ✅ Complete / ⚠️ Issues Found / ❌ Broken / 🚧 Stubbed

**Issues Found:**

1. **[Issue Type]:** [Description]
   - **Severity:** Critical / High / Medium / Low
   - **Location:** [File path, line number]
   - **Impact:** [What users experience]
   - **Recommendation:** [How to fix]

**Details:**
- Component file: `path/to/component.tsx`
- API calls: [List of API endpoints used]
- Dependencies: [Other screens/components this depends on]

---

## Issue Summary by Severity

### Critical Issues
- [List critical issues]

### High Priority Issues
- [List high priority issues]

### Medium Priority Issues
- [List medium priority issues]

### Low Priority Issues
- [List low priority issues]

---

## Recommendations

1. [Priority recommendation]
2. [Priority recommendation]
3. [Priority recommendation]

---

## Next Steps

- [ ] Create feature requests for critical issues
- [ ] Update documentation for discovered gaps
- [ ] Prioritize fixes based on user impact
```

---

### Step 5: Classify Issues

Classify each issue found:

#### Severity Levels

**Critical:**
- Screen completely broken (doesn't render, crashes)
- Security vulnerabilities (unauthorized access, data leaks)
- Data loss risks (unsaved data, incorrect data storage)
- Blocks core user flows

**High Priority:**
- Major functionality missing or broken
- Incorrect data display
- Navigation issues that confuse users
- API integration failures

**Medium Priority:**
- Minor functionality issues
- UI/UX problems (layout, styling)
- Performance issues (slow loading, lag)
- Missing error handling

**Low Priority:**
- Cosmetic issues (typos, minor styling)
- Non-critical missing features
- Optimization opportunities
- Documentation gaps

---

### Step 6: Verify Against Documentation

Cross-reference findings with existing documentation:

1. **Check feature requests:**
   - Are identified issues already tracked in the GitHub Project (Feature Requests view, label `coordination:feature-request`)?
   - Are stubbed screens documented as incomplete features?

2. **Check architecture docs:**
   - Do screens match documented flows in `docs/04-behavior/`?
   - Are API endpoints documented in `docs/05-integrations/`?

3. **Check system map:**
   - Are screens referenced in architecture documentation?
   - Are missing screens documented?

---

### Step 7: Create Action Items

For each critical or high-priority issue:

1. **Create feature request** (if not exists):
   - Create Project item with label `coordination:feature-request`
   - Include in item body: Description, severity, impact, recommended fix

2. **Update documentation:**
   - Update architecture docs if screen structure changed
   - Update flow docs if user flows are broken
   - Update API docs if endpoints are incorrect

3. **Note in status tracking:**
   - Add to Project Feature Requests view if it's a feature gap
   - Add to `CURRENT_STATUS.md` if it's a known issue

---

## Common Patterns to Detect

### Stub Patterns

**Web (React/TypeScript):**
```typescript
// Stub indicators:
<div>TODO: Implement this</div>
<button disabled>Coming soon</button>
const data = mockData; // Should be API call
// FIXME: This is incomplete
```

**Mobile (Flutter/Dart):**
```dart
// Stub indicators:
Text('TODO: Implement this'),
ElevatedButton(
  onPressed: null, // Disabled button
  child: Text('Coming soon'),
),
final data = mockData; // Should be API call
// FIXME: This is incomplete
```

### Broken Pattern Indicators

- Components that import non-existent modules
- API calls to undefined endpoints
- Navigation to non-existent routes
- References to deleted components
- Missing environment variables
- Hardcoded values that should be configurable

---

## Tools and Commands

### Web Application

**Check for broken imports:**
```bash
npm run build  # Will fail on broken imports
npm run lint   # May catch some issues
```

**Check routes:**
```bash
grep -r "Route\|route" src/app/
```

**Find stub patterns:**
```bash
grep -r "TODO\|FIXME\|STUB\|PLACEHOLDER" src/
```

### Mobile Application

**Check for broken imports:**
```bash
flutter analyze  # Will catch import errors
flutter build ios --no-codesign  # Will fail on broken imports
```

**Find stub patterns:**
```bash
grep -r "TODO\|FIXME\|STUB\|PLACEHOLDER" lib/
```

---

## Reporting Format

When reporting findings, use this format:

```markdown
### [Screen Name]

**Route/Path:** `/path/to/screen`
**Component:** `components/role/ComponentName.tsx`
**Status:** ⚠️ Issues Found

**Issues:**

1. **Stubbed API Call** (High Priority)
   - **Location:** `ComponentName.tsx:45`
   - **Issue:** Uses `mockData` instead of API call
   - **Impact:** Screen displays fake data, not real data
   - **Fix:** Replace with `fetch('/api/endpoint')`

2. **Broken Navigation** (Medium Priority)
   - **Location:** `ComponentName.tsx:120`
   - **Issue:** Link to `/old-route` (route doesn't exist)
   - **Impact:** Users get 404 error
   - **Fix:** Update to `/new-route`
```

---

## Review Checklist

Before completing the examination:

- [ ] All screens in routing config have been examined
- [ ] All findings are documented with severity levels
- [ ] Critical issues have feature requests created
- [ ] Documentation has been updated to reflect findings
- [ ] Findings report is saved in `docs/08-status/screen-examination/`
- [ ] Summary added to `CURRENT_STATUS.md` or Project Feature Requests view

---

## Integration with Other Processes

This SOP integrates with:

1. **Feature Request Process:** Critical findings should become feature requests
2. **Documentation Process:** Findings should update relevant architecture/behavior docs
3. **QA Process:** Findings can inform test cases
4. **Status Tracking:** Findings should be reflected in status documents

---

## Notes

- **Don't fix issues during examination:** Document first, fix later (unless explicitly requested)
- **Focus on user-facing issues:** Prioritize issues that affect end users
- **Verify with actual usage:** If possible, test screens in a running application
- **Consider context:** Some "stubs" may be intentional placeholders for future features

---

**Last Updated:** 2026-01-17
