# Website and App Content

**Purpose:** Single location for **page-level and marketing content** for the website and app (homepage, landing pages, section intros, etc.). These are the **source-of-truth copies** used for implementation, review, and localization. This folder does not replace the design system or UI copy embedded in components; it holds **full-page or section content** that is typically authored by product/marketing and consumed by the app or CMS.

**Location:** `docs/06-operations/content/`

**Management:** Follow [Content Documents SOP](../CONTENT_DOCUMENTS_SOP.md) when adding, updating, or retiring content documents.

**When to use:** Before creating or changing homepage, landing, or key marketing content; when handing off copy to frontend or CMS; when tracking what content exists per page or section.

**Typical structure (add per project):**
- `HOME.md` or `HOMEPAGE.md` — Homepage hero, headlines, and section copy
- `ABOUT.md` — About page content
- `[SECTION_NAME].md` — One file per major page or section (e.g. `EVENTS.md`, `CANDIDATES.md`, `DONATE.md`)
- Subfolders (e.g. `content/landing/`, `content/email/`) only if the project adopts them and the Content Documents SOP defines them

**Naming:** Use `UPPER_SNAKE_CASE.md` for document names, or `lowercase-with-hyphens` if the project standard differs (document in Content Documents SOP). One file per page or section unless the SOP specifies otherwise.

---

**Last Updated:** 2026-01-27
