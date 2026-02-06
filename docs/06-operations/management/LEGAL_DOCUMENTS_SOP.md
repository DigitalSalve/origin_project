# Legal Documents — Management SOP

**Purpose:** Defines how to manage the legal documents folder (`docs/06-operations/legal/`), including adding, updating, and retiring legal documents (terms of service, privacy policy, cookie policy, disclaimers, etc.). Use when creating or changing any legal document or when publishing or linking legal text in the app or website.

**Status:** Active

**Applies To:** Product owners, legal/compliance, content authors, and AI agents creating or updating legal documents.

---

## Overview

This SOP ensures that:

1. **Single location** — All legal documents live in `docs/06-operations/legal/` and are the source of truth for published legal text.
2. **Consistent naming and structure** — File names and internal structure (headings, sections) follow project conventions so documents are discoverable and maintainable.
3. **Controlled changes** — Add, update, and retire legal documents only after following this process; versioning or effective-date notes are recorded when required.
4. **Traceability** — Changes are attributable (who, when, why) and linked from the doc or the README in the legal folder.

---

## Prerequisites

Before adding, updating, or retiring a legal document:

1. **Read relevant documentation:**
   - `AI.md` and `docs/README.md`
   - [Legal folder README](legal/README.md)
   - [File Naming Conventions](FILE_NAMING_CONVENTIONS.md) (folder and file naming)
   - [Legal Documents SOP](LEGAL_DOCUMENTS_SOP.md) (this doc)

2. **Understand scope:**
   - **In scope:** Terms of service, privacy policy, cookie policy, disclaimers, contributor agreements, accessibility statements, and other project-defined legal/compliance documents.
   - **Out of scope:** Legal documents that must live elsewhere (e.g. contract templates in a legal repo); follow project canon if a different location is mandated.

3. **Identify change type:** New document, update to existing document, or retirement. Use the matching process step below.

---

## Process

### Step 1: Add a New Legal Document

Use when the project is introducing a new legal document type (e.g. first privacy policy, new cookie policy).

#### 1.1 Decide name and purpose

- [ ] **Choose document type:** Terms of service, privacy policy, cookie policy, disclaimers, or other (document in legal README).
- [ ] **Name the file:** Use `UPPER_SNAKE_CASE.md` (e.g. `TERMS_OF_SERVICE.md`, `PRIVACY_POLICY.md`, `COOKIE_POLICY.md`). Match [File Naming Conventions](FILE_NAMING_CONVENTIONS.md) for documentation where applicable.
- [ ] **Confirm ownership:** Identify who approves legal content (product, legal, compliance). Do not publish or link from the app until approved per project policy.

#### 1.2 Create the file in the legal folder

- [ ] **Create** the file under `docs/06-operations/legal/` with the chosen name.
- [ ] **Include** a short header: purpose, effective date (if applicable), and “Last updated” (YYYY-MM-DD).
- [ ] **Use clear structure:** Top-level headings for major sections (e.g. “1. Scope”, “2. Data We Collect”, “3. How We Use Data”). Keep formatting consistent with existing legal docs in the folder.
- [ ] **Do not** store draft-only or internal legal advice in this folder if project policy restricts it; use a different location for drafts and link from the legal README if needed.

#### 1.3 Register and link

- [ ] **Update** `docs/06-operations/legal/README.md`: add the new file to the “Typical files” or “Documents in this folder” list with a one-line description.
- [ ] **Add to docs/README:** If the repo spine lists legal docs, add a link in Layer 6 (Operations) to the legal folder or to the new doc.
- [ ] **Link from app/website:** When the document is published, ensure the in-app or in-site link points at the canonical location (e.g. rendered from this file or from a single source that is synced from this file). Document the publishing path in the legal README or in a project runbook if needed.

**Output:** New legal document in `docs/06-operations/legal/`, listed in legal README, and linked from docs/README if applicable. No broken links.

---

### Step 2: Update an Existing Legal Document

Use when changing the text of an existing legal document (e.g. privacy policy update, new clause in terms).

#### 2.1 Identify what is changing

- [ ] **Open** the existing file in `docs/06-operations/legal/`.
- [ ] **Decide** scope: full revision, new section, or specific clause/sentence change. Ensure changes are approved per project policy (legal/compliance/product).
- [ ] **Effective date:** If the document has an effective date, decide whether this change requires a new effective date and update the header accordingly.

#### 2.2 Apply the change

- [ ] **Edit** the file in place. Preserve existing structure unless a restructure is explicitly approved.
- [ ] **Update** the “Last updated” (and “Effective date” if applicable) in the header.
- [ ] **Optional:** Add a short “Change log” or “Revisions” section at the end of the document (e.g. “2026-01-27: Updated data retention section”). If the project requires a change log, make it a standard part of every update.

#### 2.3 Record and communicate

- [ ] **Record** who made the change and when (in the doc, in a commit message, or in a change log — per project practice).
- [ ] **If user-facing:** Plan communication or in-app notice (e.g. banner, email) per project policy. Document in legal README or runbook if this SOP does not cover comms.
- [ ] **Publish:** Ensure the live app/website reflects the updated document (publish from this source or sync from it). Confirm links still resolve.

**Output:** Updated legal document with current “Last updated” (and effective date if used); change attributable; live experience updated if applicable.

---

### Step 3: Retire or Replace a Legal Document

Use when removing a legal document type or replacing it with another (e.g. merging two policies, sunsetting a deprecated policy).

#### 3.1 Confirm retirement or replacement

- [ ] **Confirm** with product/legal/compliance that the document is to be retired or fully replaced.
- [ ] **If replacing:** Ensure the replacement document exists and is approved before retiring the old one. Follow Step 1 for the new document, then Step 3.2 for the old one.
- [ ] **Redirects or notices:** Decide whether users should be redirected to a new doc or see a short “This document is no longer in effect” notice. Document the decision and implement in the app/website.

#### 3.2 Remove or archive the file

- [ ] **Retire:** Remove the file from `docs/06-operations/legal/` only after the replacement or notice is in place and links in the app/website are updated.
- [ ] **Or archive:** If the project keeps a history of legal doc versions, move the retired file to an `archive/` or `retired/` subfolder under `legal/` with a dated name (e.g. `archive/PRIVACY_POLICY_2024-01-15.md`). Document the archive location and retention in the legal README.
- [ ] **Update** `docs/06-operations/legal/README.md`: remove the retired doc from the active list; if archived, add one line under an “Archived” or “Retired” section with path and date.

#### 3.3 Update links and docs

- [ ] **Update** any link in docs/README, legal README, or runbooks that pointed at the retired document. Point to the replacement or to the archive, as appropriate.
- [ ] **Update** the app/website so no link points at the retired doc; use redirect or notice as decided in 3.1.

**Output:** Retired document removed or archived; README and links updated; app/website updated so users do not see broken or outdated legal links.

---

### Step 4: Keep Structure and Naming Consistent

Use whenever adding or renaming files so the legal folder stays organized and discoverable.

#### 4.1 Naming

- [ ] **Files:** `UPPER_SNAKE_CASE.md` (e.g. `TERMS_OF_SERVICE.md`, `PRIVACY_POLICY.md`). No spaces or special characters. Match [File Naming Conventions](FILE_NAMING_CONVENTIONS.md) where it addresses docs in operations.
- [ ] **One document type per file:** One terms doc, one privacy doc, etc. Do not merge unrelated legal docs into a single file unless the project explicitly adopts a “combined policy” and documents it in the legal README.

#### 4.2 Structure inside documents

- [ ] **Header:** Purpose (one line), effective date (if used), last updated (YYYY-MM-DD).
- [ ] **Sections:** Numbered or named top-level headings so table-of-contents and deep links work. Keep style consistent across legal docs in the folder.
- [ ] **Change log:** If the project requires it, maintain a “Revisions” or “Change log” section at the end of each doc.

#### 4.3 README and discoverability

- [ ] **Legal README:** Lists every active document with name and one-line description. Updated whenever a doc is added, retired, or archived.
- [ ] **docs/README (Layer 6):** Links to the legal folder (or to key legal docs) so the legal folder is discoverable from the documentation spine.

**Output:** Legal folder and its files follow a single, documented structure; new contributors can find and update legal docs without guessing.

---

## Integration with Other Processes

- **File Naming Conventions:** Legal file and folder names follow this SOP and align with [FILE_NAMING_CONVENTIONS.md](FILE_NAMING_CONVENTIONS.md) for documentation.
- **Documentation Process:** When a legal doc is part of “documentation” that must be kept in sync with releases, follow [DOCUMENTATION_PROCESS.md](../processes/DOCUMENTATION_PROCESS.md) for versioning or release checklist items that mention legal.
- **Canon / Compliance:** If canon or compliance rules mandate where legal text lives or who approves it, this SOP does not override them. Follow canon first; use this SOP for how to manage the `legal/` folder and its files.
- **App/Website:** Legal documents in this folder are the source of truth. Document in the legal README or in a runbook how they are published (e.g. static page, CMS import, build-time copy).

---

## Notes

- **Approval:** This SOP does not define who must approve legal content. The project must define that (product, legal, compliance) and enforce it outside this SOP.
- **Localization:** If legal documents are localized, define in the legal README whether each locale has its own file (e.g. `PRIVACY_POLICY_en.md`, `PRIVACY_POLICY_es.md`) or a single file with embedded locales. Keep the SOP process the same; only file-naming or structure may change.
- **Drafts:** If drafts live outside the legal folder, say so in the legal README and point to where they live. Only approved, published (or about-to-be-published) content should be in `docs/06-operations/legal/` as the source of truth.

---

**Last Updated:** 2026-01-27
