---
name: visual-check
description: Screenshot the pages a UI change touches, before and after, at phone and desktop width in light and dark mode, then compare them. Use after changing layout, styles or components, before opening a PR for a UI change, when the user reports something looks broken, or to check a preview/live deploy. Catches overflow, overlap, clipped text and broken themes that tests miss.
---

# Visual check

Claude edits UI it can't see. This skill makes it look.

## Project facts

The dev server command and its local URL are in the `## Dev` section of
`DOCS/running.md` (`DOCS` is the docs folder the project's `CLAUDE.md`
names, usually `vault/`), and the production domain in `## Deploy`. If they
are missing, ask the user once and write them there.

## Setup (once per machine)

The project doesn't need Playwright as a dependency; run it through `npx`:

```
npx -y playwright@latest install chromium
```

If the project already has `@playwright/test`, use its version instead
(`npx playwright ...`) so the browsers match.

Screenshots go in `.visual-check/` at the repo root. Add it to `.gitignore`
the first time; screenshots never get committed.

## Steps

1. **Pick the pages.** Only the routes the change can affect: the page
   edited, plus every page using a changed component or shared style. A
   change to a global stylesheet or layout means the main page of each page
   type.

2. **Take "before" shots** of the unchanged code. Best: run them before
   editing. If the change is already made, check out the base branch in a
   worktree and run its dev server on another port:
   ```
   git worktree add ../_visual-before main
   ```
   (remove it afterwards with `git worktree remove ../_visual-before`). For a
   deployed site, the live or preview URL works as "before" too.

3. **Take "after" shots** from the dev server with the change. For every
   page, four shots: phone (390×844) and desktop (1440×900), light and dark:
   ```
   npx -y playwright@latest screenshot --full-page --viewport-size=390,844 --color-scheme=dark \
     http://localhost:3000/pricing .visual-check/after/pricing-phone-dark.png
   ```
   Name files `<page>-<phone|desktop>-<light|dark>.png` in `before/` and
   `after/` so pairs line up. Add `--wait-for-selector` or
   `--wait-for-timeout=1000` if the page animates in or loads data.

4. **Look at every pair** (read the images) and check for:
   - horizontal scroll at phone width, or content cut off at an edge
   - overlapping elements, text clipped by its container, wrapped buttons
   - the theme breaking: dark text on dark ground, light surfaces left in
     dark mode, invisible borders or icons
   - spacing or alignment that changed where it shouldn't have
   - layout shift from images without dimensions, fonts not loading
   For a large page where differences are hard to spot by eye, ImageMagick
   can point at them: `compare before.png after.png diff.png`.

5. **Report** per page: what changed on purpose (and looks right), what
   changed by accident, and anything broken, with the file names of the
   shots that show it. Fix accidental breakage before calling the change
   done. When the user should see it themselves, say which shots to open.

## Notes

- Don't screenshot every page of the site for a one-component change; the
  point is a quick, targeted look.
- Pages behind a login need a session: use the project's E2E auth setup if
  there is one, or ask the user for a test account rather than skipping it.
- Things a screenshot can't judge (how an animation feels, hover and focus
  states) go on the issue as a checkbox for the user.
