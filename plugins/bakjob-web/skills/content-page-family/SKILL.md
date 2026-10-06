---
name: content-page-family
description: Use when building or editing one page in a family of similar structured pages (e.g. case studies, product pages, doc pages for different APIs) that share a narrative/visual structure but differ in content. Encodes the shared structure, style conventions, and how to handle content that's still TBC from stakeholders.
---

# Content page family

A common pattern: a project has several pages that are structurally identical but content-different — case studies, portfolio pieces, product detail pages, per-API doc pages.

## Project facts

The family itself is described in `DOCS/design/page-families.md` (`DOCS` is the docs folder the project's `CLAUDE.md` names, usually `vault/`). Per family it holds:

- a table with each page's route and its section arc, e.g. `/work/acme`: problem → approach → result chart → quote;
- shared conventions: chart/visual library, data-source notes;
- the brand tokens (colors, fonts) used across the family.

If the file or the family is missing, build the first page, then write down its route, arc and conventions there so the next page matches it. That file is a structural reference; check the source of truth (issue tracker, CMS, brief doc) for the latest content before writing copy.

## Building a page

- Follow the arc and conventions from `page-families.md`; a new page should be recognizably the same kind of page as its siblings.
- The index page (if one exists) is typically a card grid — one card per item with title, one-line summary, a tag, and a link, filterable by category. Build new items so they slot into the existing grid without a layout redesign.
- If content uses anonymised/non-live data, state that plainly rather than implying it's live.
- Add the new page's row to the table in `page-families.md`.

## Placeholder / TBC content

Some pages in the family may be explicitly placeholder while full content is pending from stakeholders. When building these:
- Write structurally complete pages using clearly fictional/placeholder numbers, not real specifics you don't have.
- Mark the placeholder status in a comment at the top of the page component (not visible copy) so it isn't mistaken for final content.
- Don't invent identifying details you don't have permission to invent (real names, real client data) — keep placeholders generic.
