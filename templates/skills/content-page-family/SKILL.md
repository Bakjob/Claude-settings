---
name: content-page-family
description: Use when building or editing one page in a family of similar structured pages (e.g. case studies, product pages, doc pages for different APIs) that share a narrative/visual structure but differ in content. Encodes the shared structure, style conventions, and how to handle content that's still TBC from stakeholders.
---

# Content page family

*(This is a template for a common pattern: a project has several pages that are structurally identical but content-different — case studies, portfolio pieces, product detail pages, per-API doc pages, etc. Fill in the specifics below for the actual family this project has.)*

Check the source of truth (issue tracker, CMS, brief doc) for the latest content before writing copy — this file is a structural reference, not the source of truth for content.

## Routes and structure

*(List each page in the family: route, and its narrative/section arc. Example shape:)*

| Page | Route | Arc |
|---|---|---|
| [Name] | `/[family]/[slug]` | [Section 1] → [Section 2] → [Section 3] → [Section 4]. [Any distinguishing layout element, e.g. a chart, a before/after table.] |

The index page (if one exists) is typically a card grid — one card per item with title, one-line summary, a tag, and a link, filterable by category. Build new items so they slot into the existing grid without a layout redesign.

## Shared conventions

- Chart/visual library: [WHATEVER THIS PROJECT USES].
- Brand colors/type used across the family: [TOKENS].
- Data source notes: if content uses anonymised/non-live data, state that plainly rather than implying it's live.

## Placeholder / TBC content

Some pages in the family may be explicitly placeholder while full content is pending from stakeholders. When building these:
- Write structurally complete pages using clearly fictional/placeholder numbers, not real specifics you don't have.
- Mark the placeholder status in a comment at the top of the page component (not visible copy) so it isn't mistaken for final content.
- Don't invent identifying details you don't have permission to invent (real names, real client data) — keep placeholders generic.

## Brand tokens (shared across the family)

*(Colors, fonts, whatever this project's design system defines.)*

---
*Template — fill in the routes/arc table, shared conventions, and brand tokens for the target project's actual page family.*
