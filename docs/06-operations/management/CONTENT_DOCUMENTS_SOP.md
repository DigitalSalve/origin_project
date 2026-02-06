# Content Documents — Management SOP

**Purpose:** Defines how to manage the content documents folder (`docs/06-operations/content/`), including adding, updating, and retiring page-level and marketing content (homepage, landing pages, section intros, etc.). Use when creating or changing website or app content that is authored as full-page or section copy and consumed by the app or CMS.

**Status:** Active

**Applies To:** Product owners, content authors, marketing, and AI agents creating or updating website or app page content.

---

## Overview

This SOP ensures that:

1. **Single location** — Page-level and marketing content lives in `docs/06-operations/content/` and is the source of truth for what appears on homepage, landing, and key sections.
2. **Consistent naming and structure** — File names and internal structure (headings, sections, components) follow project conventions so content is discoverable and maintainable.
3. **Controlled changes** — Add, update, and retire content documents only after following this process; ownership and “where this appears in the app” are clear.
4. **Traceability** — Changes are attributable (who, when, why) and linked from the doc or the README in the content folder.

---

## Prerequisites

Before adding, updating, or retiring a content document:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - [Content folder README](content/README.md)
   - [File Naming Conventions](FILE_NAMING_CONVENTIONS.md) (folder and file naming)
   - [Content Documents SOP](CONTENT_DOCUMENTS_SOP.md) (this doc)
   - [Design System](../04-behavior/DESIGN_SYSTEM.md) or project copy guidelines if they exist (tone, structure, components).

2. **Understand scope:**
   - **In scope:** Homepage, landing pages, section intros, marketing headlines, long-form page copy, and other project-defined “content” that is authored as documents and consumed by the app or CMS. This folder does not replace the design system or UI strings in code; it holds **full-page or section content** that is typically owned by product/marketing.
   - **Out of scope:** UI strings embedded in components, legal text (use `docs/06-operations/legal/` per [Legal Documents SOP](LEGAL_DOCUMENTS_SOP.md)), or content that must live in a CMS or other system by project choice — in that case, document where it lives and how this folder relates to it.

3. **Identify change type:** New content document, update to existing content, or retirement. Use the matching process step below.

---

## Process

### Step 1: Add a New Content Document

Use when the project is introducing a new page or section that has its own authored content (e.g. first homepage content, new landing page).

#### 1.1 Decide name, purpose, and ownership

- [ ] **Choose page or section:** Homepage, about, donate, events, candidates, or other (document in content README).
- [ ] **Name the file:** Use `UPPER_SNAKE_CASE.md` (e.g. `HOMEPAGE.md`, `ABOUT.md`, `DONATE.md`) unless the project adopts a different convention — then document it in the content README and follow it consistently.
- [ ] **Define “where it appears”:** Which route, screen, or CMS slot will consume this content. Note it in the document header or in the content README so implementers know where to wire it.
- [ ] **Confirm ownership:** Identify who approves content (product, marketing). Do not publish or link from the app until approved per project policy.

#### 1.2 Create the file in the content folder

- [ ] **Create** the file under `docs/06-operations/content/` with the chosen name.
- [ ] **Include** a short header: purpose, “Where used” (route/screen/CMS identifier), and “Last updated” (YYYY-MM-DD).
- [ ] **Use clear structure:** Top-level headings for major sections (e.g. “Hero”, “Featured”, “Section 1”, “Section 2”). If the project uses a standard content template (headline, subhead, body, CTA), follow it so parsing or CMS mapping is consistent.
- [ ] **Optional:** Add placeholders for images, links, or dynamic data (e.g. “{candidate_name}”, “{event_list}”) and document their meaning in the header or a “Variables” section so implementers know how to bind them.

#### 1.3 Register and link

- [ ] **Update** `docs/06-operations/content/README.md`: add the new file to the “Typical structure” or “Documents in this folder” list with a one-line description and “Where used.”
- [ ] **Add to docs/README:** If the repo spine lists content docs, add a link in Layer 6 (Operations) to the content folder or to the new doc.
- [ ] **Wire to app/website:** When the content is implemented, ensure the consuming route/screen/CMS uses this file (or a single source synced from it) as the source of truth. Document the publishing path in the content README or in a project runbook if needed.

**Output:** New content document in `docs/06-operations/content/`, listed in content README with “Where used,” and linked from docs/README if applicable. No broken links.

---

### Step 2: Update an Existing Content Document

Use when changing the copy or structure of an existing page or section (e.g. new homepage hero, revised about copy).

#### 2.1 Identify what is changing

- [ ] **Open** the existing file in `docs/06-operations/content/`.
- [ ] **Decide** scope: full rewrite, new section, or specific block (headline, body, CTA). Ensure changes are approved per project policy (product/marketing).
- [ ] **Impact:** Confirm which route/screen/CMS uses this doc so implementers can update or redeploy as needed.

#### 2.2 Apply the change

- [ ] **Edit** the file in place. Preserve existing structure unless a restructure is explicitly approved and reflected in the app/CMS.
- [ ] **Update** the “Last updated” in the header.
- [ ] **Optional:** Add a short “Change log” or “Revisions” section at the end (e.g. “2026-01-27: Updated hero headline and CTA”). If the project requires a change log, make it a standard part of every update.
- [ ] **Variables/placeholders:** If you add or remove placeholders, document them in the header or “Variables” section and notify implementers so binding stays correct.

#### 2.3 Record and communicate

- [ ] **Record** who made the change and when (in the doc, in a commit message, or in a change log — per project practice).
- [ ] **Publish:** Ensure the live app/website reflects the updated content (rebuild, CMS update, or sync from this source). Confirm the right route/screen shows the new copy.

**Output:** Updated content document with current “Last updated”; change attributable; live experience updated when implemented.

---

### Step 3: Retire or Replace a Content Document

Use when removing a page/section or replacing it with another (e.g. retiring a landing page, merging two sections into one).

#### 3.1 Confirm retirement or replacement

- [ ] **Confirm** with product/marketing that the content is to be retired or fully replaced.
- [ ] **If replacing:** Ensure the replacement document exists and is approved before retiring the old one. Follow Step 1 for the new document, then Step 3.2 for the old one.
- [ ] **Redirects or fallbacks:** Decide whether the route/screen should redirect, show a generic “Page no longer available,” or show new content. Document the decision and implement in the app/website.

#### 3.2 Remove or archive the file

- [ ] **Retire:** Remove the file from `docs/06-operations/content/` only after the replacement or fallback is in place and the app/website no longer references the old content.
- [ ] **Or archive:** If the project keeps a history of content versions, move the retired file to an `archive/` or `retired/` subfolder under `content/` with a dated name (e.g. `archive/HOMEPAGE_2024-06-01.md`). Document the archive location in the content README.
- [ ] **Update** `docs/06-operations/content/README.md`: remove the retired doc from the active list; if archived, add one line under an “Archived” or “Retired” section with path and date.

#### 3.3 Update links and app/website

- [ ] **Update** any link in docs/README, content README, or runbooks that pointed at the retired document. Point to the replacement or to the archive, as appropriate.
- [ ] **Update** the app/website so no route/screen/CMS still points at the retired content; use redirect or new content as decided in 3.1.

**Output:** Retired content removed or archived; README and links updated; app/website updated so users do not see broken or stale content.

---

### Step 4: Keep Structure and Naming Consistent

Use whenever adding or renaming files so the content folder stays organized and discoverable.

#### 4.1 Naming

- [ ] **Files:** `UPPER_SNAKE_CASE.md` (e.g. `HOMEPAGE.md`, `ABOUT.md`, `DONATE.md`) unless the project adopts a different convention — then document it in the content README.
- [ ] **One page or section per file:** One homepage doc, one about doc, etc. Do not merge unrelated pages into a single file unless the project explicitly adopts a “combined content” file and documents it in the content README.
- [ ] **Subfolders:** Use subfolders (e.g. `content/landing/`, `content/email/`) only if the project defines them in the content README and this SOP. Otherwise keep all content files in `docs/06-operations/content/`.

#### 4.2 Structure inside documents

- [ ] **Header:** Purpose (one line), “Where used” (route/screen/CMS), last updated (YYYY-MM-DD).
- [ ] **Sections:** Clear headings for each block (Hero, Featured, Section 1, etc.) so implementers and authors can map content to UI or CMS slots. Keep style consistent across content docs.
- [ ] **Variables/placeholders:** If used, list them in the header or a “Variables” section and keep the list up to date when copy changes.
- [ ] **Change log:** If the project requires it, maintain a “Revisions” or “Change log” section at the end of each doc.

#### 4.3 README and discoverability

- [ ] **Content README:** Lists every active document with name, one-line description, and “Where used.” Updated whenever a doc is added, retired, or archived.
- [ ] **docs/README (Layer 6):** Links to the content folder (or to key content docs) so the content folder is discoverable from the documentation spine.

**Output:** Content folder and its files follow a single, documented structure; new contributors can find and update content without guessing.

---

## Integration with Other Processes

- **File Naming Conventions:** Content file and folder names follow this SOP and align with [FILE_NAMING_CONVENTIONS.md](FILE_NAMING_CONVENTIONS.md) for documentation.
- **Design System / Behavior:** Content tone and structure should align with [Design System](../04-behavior/DESIGN_SYSTEM.md) and any project copy guidelines. This SOP governs **where** and **how** content is stored and updated; it does not override design or product copy rules.
- **Legal Documents:** Legal text (terms, privacy, etc.) lives in `docs/06-operations/legal/` per [Legal Documents SOP](LEGAL_DOCUMENTS_SOP.md). Do not put legal documents in the content folder.
- **Documentation Process:** When content is part of “documentation” that must be kept in sync with releases, follow [DOCUMENTATION_PROCESS.md](../processes/DOCUMENTATION_PROCESS.md) for any versioning or release checklist items that mention content.
- **App/Website:** Content in this folder is the source of truth. Document in the content README or in a runbook how it is consumed (e.g. static build, CMS import, runtime fetch).

---

## Notes

- **Approval:** This SOP does not define who must approve content. The project must define that (product, marketing) and enforce it outside this SOP.
- **Localization:** If content is localized, define in the content README whether each locale has its own file (e.g. `HOMEPAGE_en.md`, `HOMEPAGE_es.md`) or a single file with embedded locales. Keep the SOP process the same; only file-naming or structure may change.
- **CMS vs. repo:** If the project uses a CMS as the primary store for live content, define in the content README whether this folder is the source that feeds the CMS, a mirror for review, or used only for initial seed content. The SOP still applies to how files in this folder are added, updated, and retired.

---

**Last Updated:** 2026-01-27
