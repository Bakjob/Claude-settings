# Project facts: where the bakjob skills look things up

The skills and agents in `bakjob-core`, `bakjob-github`, `bakjob-linear` and
`bakjob-web` are shared by every project that enables them, so they hold no
project-specific values. They read them from the project's own files, in the
places listed here. Bootstrap writes these files; [generate.md](generate.md)
must produce everything below that applies to the project.

Every skill follows the same rule when a fact is missing: **ask the user
once, then write the answer into the file named here**, so the next session
finds it. That also makes the skills usable in a project that was never
bootstrapped.

## `CLAUDE.md` (repo root)

- **First heading:** the project name. **First paragraph:** what it is and
  its stack.
- **The docs folder** (`DOCS` below), named in the "Project docs" section.
  Default `vault/` when it isn't named.
- **"Issue tracker" section:** which tracker, and its identifiers:
  - GitHub Issues: the repo as `owner/name` (if absent, read
    `git remote get-url origin`).
  - Linear: workspace slug, team, project, issue prefix, and the foundational
    blocker issues once they exist.
- **"Git workflow" section:** branch naming, who merges, what Claude may do
  without asking.

## `DOCS/hard-rules.md`

The enforced rules, short. Optional line used by `config-value-auditor`:

```markdown
**Config file:** `src/config.ts`. Every tunable value lives there:
durations, thresholds, sizes, rates, costs, limits.
```

## `DOCS/running.md`

One section per command group, each with the exact command:

- `## Setup`, `## Dev` (the dev server command and its local URL, used by
  `visual-check`), `## Build`
- `## Test`: frameworks (unit, E2E) and commands, plus type/lint checks
- `## Lint and format`
- `## Smoke test`: command, run-length options or flags, any prep step
  needed first, and one line per check: its name, its pass criterion, and
  the file or subsystem it points at when it fails
- `## Deploy`: for games, the release targets instead: itch.io `user/game`
  and channels, Steam app and depot IDs and where the SteamPipe scripts live,
  export presets per platform, version scheme (used by `game-release`). For
  web: platform or host, the project/app name on it, production
  domain and branch, the environment variable names (never values) and
  where they're set, deploy and rollback steps, and post-launch steps
  (search console). Used by `static-site-deploy` and `web-deploy`.

## `DOCS/quality-targets.md` (web projects)

The SEO, accessibility and performance bar `seo-a11y-auditor` checks
against: Lighthouse thresholds and which pages, WCAG version and level,
meta title convention, JSON-LD schemas per page type, and where the targets
came from (a brief, an issue).

## `DOCS/quality-targets.md` (game projects)

The bar `performance-auditor` checks against: target platforms and minimum
spec, target frame rate (the frame budget follows from it), memory and
load-time budgets. An `## Accessibility` part holds the target tier
(Basic / Intermediate / Advanced, Game Accessibility Guidelines) and any
platform requirement, used by `game-accessibility`.

## `DOCS/design/page-families.md` (web projects with repeated page types)

Per family of similar pages (case studies, product pages): a table of route
and section arc per page, the shared conventions (chart library, data
notes) and the brand tokens. Used by `content-page-family`.

## The vault itself

`DOCS/decisions/` (numbered, with an index in `decisions/README.md`),
`DOCS/progress/` (dated entries), `DOCS/status/current-status.md` and
`DOCS/status/open-questions.md`. `DOCS/README.md` explains the layout and
may mark files as human-owned.
