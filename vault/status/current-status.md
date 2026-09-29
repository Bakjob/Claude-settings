# Current status

Last updated: 2026-09-29

## What this repo is

A personal library of reusable Claude Code configuration: a `CLAUDE.md`
template, custom agents, and custom skills, meant to be copied (in whole or
in part) into new project repos and then filled in / trimmed per project.
See the root `README.md` for the full explanation and usage instructions.

## What's in the library right now

**`CLAUDE-template.md`** — the root-level project instructions template.
Already fully generic (bracket placeholders throughout); no rework needed in
this pass.

**`agents/`** (6, all Claude-Code subagents, all templated):
- `architect.md` — plans hard, multi-subsystem problems before code is written
- `config-value-auditor.md` — finds hardcoded values that should live in a central config file
- `seo-a11y-auditor.md` — audits a site against SEO/accessibility/performance targets
- `smoke-test-runner.md` — runs and interprets a project's smoke test
- `test-runner.md` — writes and runs automated tests, verifies the build
- `vault-scribe.md` — keeps a project's `vault/` up to date after real work

**`skills/`** (11):
- `git-checkpoint` — templated: checks whether now is a good commit/PR point
- `linear-dev-workflow` — **kept concrete** (real Linear MCP calls), only identifiers templated — see [decision 0003](../decisions/0003-linear-stays-concrete.md)
- `static-site-deploy` (was `loopia-deploy`) — templated static-hosting deploy checklist
- `new-decision` — templated: scaffolds a new `vault/decisions/` entry
- `smoke-test` — templated: runs a project's smoke test and reads the output
- `test-all-branches` — templated: combines every open PR into one disposable local branch
- `vault-update` — templated: routine vault maintenance after a work session
- `content-page-family` (was `case-study-page`) — templated: pattern for a family of structurally-similar, content-different pages
- `design-taste-frontend` — already fully generic (anti-slop frontend design guidance); untouched this pass
- `redesign-skill` — already fully generic (redesign audit checklist); untouched this pass
- `artifact-question-desk` — templated: the question-desk format for artifacts that need the user's input, with a working HTML template (added 2026-09-29)

## Just finished

Added `skills/artifact-question-desk`: the question-desk format for artifacts
that ask the user something, lifted from a Space Hex page the user called the
best way yet to be asked questions. See
[progress/2026-09-29-artifact-question-desk.md](../progress/2026-09-29-artifact-question-desk.md).

## Not yet done

See [open-questions.md](./open-questions.md) and [ideas/](../../vault/ideas/README.md).
