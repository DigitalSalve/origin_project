# Design System — Usage SOP

**Purpose:** Defines when and how to use `docs/04-behavior/DESIGN_SYSTEM.md`, who owns it, and how to keep it accurate as the single source for visual and interaction standards.

**Status:** Active

**Applies To:** Frontend developers, designers, and AI agents implementing or updating UI components, layouts, and role-based patterns.

---

## Overview

This SOP ensures that:

1. **When to use** — Teams use the design system when building or changing UI (components, colors, type, spacing, role-based layouts), when onboarding to visual standards, or when reviewing consistency.
2. **Who uses it** — Frontend (implementation), design (authoritative patterns), product (role-appropriate density and clarity), QA (acceptance criteria for look and feel).
3. **How to keep it updated** — Add or change tokens, components, and patterns when the product’s visual or interaction standards change; keep file locations and version history accurate.
4. **How it fits** — Layer 4 (behavior). It describes how the UI should look and behave, not data models or APIs. Architecture may reference layout/component structure; this doc defines the standards.

---

## Prerequisites

Before using or updating `DESIGN_SYSTEM.md`:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - `docs/04-behavior/DESIGN_SYSTEM.md`
   - Canon/guidelines if the project has mandated accessibility or brand constraints

2. **Understand scope:**
   - **In scope:** Color palette, typography, spacing, component patterns (buttons, cards, forms, badges, alerts), layouts by role, breakpoints, accessibility guidelines, icon usage.
   - **Out of scope:** API contracts, backend logic, state machines (those live in Layer 3/5).

3. **Note on paths:** “File Locations” (e.g. theme.css, Tailwind, component paths) are reference. For a specific app, update them to that app’s actual paths.

---

## Process

### Step 1: Decide When to Open the Doc

- [ ] **Implementing a new component or screen:** Use palette, type, spacing, and component patterns so implementation matches the system.
- [ ] **Changing visual or interaction standards:** Update the design system first, then implement; avoid one-off deviations.
- [ ] **Onboarding or review:** Use it to align team and tools on colors, type scale, role-based layouts, and accessibility.
- [ ] **Fixing consistency issues:** Use the doc as the baseline for “correct” and align code to it.

**Output:** Clear need to read or edit the design system.

---

### Step 2: Use the Doc for Your Task

- [ ] **Identify the right section:** Color Palette, Typography, Spacing, Component Library, Layout Patterns, Role-Specific Patterns, Breakpoints, Accessibility, or File Locations.
- [ ] **Apply tokens and patterns:** Use CSS variables, Tailwind classes, and code samples as specified; prefer system tokens over one-off values.
- [ ] **Respect role patterns:** Homeowner (centered, minimal), Contractor (portal/sidebar), Admin (data-dense); follow the documented density and layout for each role.
- [ ] **Check accessibility:** Contrast, focus states, touch targets, and screen-reader guidance must be followed where the doc specifies them.

**Output:** Implementation that conforms to the design system.

---

### Step 3: Update the Doc When Standards Change

- [ ] **New or changed tokens:** Update Color Palette, Typography, or Spacing; ensure “File Locations” and actual theme/tailwind files stay in sync.
- [ ] **New or changed components:** Add or update Component Library with name, usage, and code sample; keep role-specific usage clear where relevant.
- [ ] **New or changed layouts:** Update Layout Patterns and Role-Specific Patterns; document breakpoints and responsive behavior.
- [ ] **New role or product:** Add role-specific patterns and layout; update Design Philosophy if principles change.
- [ ] **File or tool change:** Update “File Locations” and any framework/tool notes (e.g. Tailwind version, config path).

**Output:** Design system remains the single source of truth for visual and interaction standards.

---

### Step 4: Keep Consistency and Traceability

- [ ] **Version history:** When making substantive changes, add or update the Version History section with a short note and date.
- [ ] **Cross-references:** If AUTHENTICATION_FLOWS or other behavior docs reference “design patterns” (e.g. role colors), keep those in sync with this doc.
- [ ] **Accessibility:** When canon or law mandates change (e.g. contrast, focus), update Accessibility Guidelines and any affected components here.

**Output:** Design system is up to date, versioned, and aligned with behavior docs and canon where applicable.

---

## Integration with Other Processes

- **AUTHENTICATION_FLOWS.md:** References “Design Patterns” and role color coding; those should match this doc’s Color Palette and Role-Specific Patterns.
- **SUBSCRIPTION_TIERS.md:** Plan-based branding/theming affects “platform-branded” vs “reduced platform branding”; design system should support those patterns when they are implemented.
- **Canon / Guidelines:** Accessibility or brand mandates in Layer 2 override local preference; this doc implements them in concrete tokens and patterns.
- **Feature notes / Guides:** When a feature changes UI standards, update this doc and reference it in the feature note.

---

## Notes

- The design system is product-agnostic in tone (e.g. “multi-role application”); when applying to a project, keep it generic or plug in the project’s name where appropriate.
- Prefer extending the system over one-off styles; if a pattern repeats, add it to the doc and use it consistently.
- “Do’s and Don’ts” and “Best Practices” are binding for implementation unless a feature request explicitly approves an exception.

---

**Last Updated:** 2026-01-27
