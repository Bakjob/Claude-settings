# Current status

Last updated: 2026-08-17

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

**`skills/`** (10):
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

## Just finished

A full pass removing leftover specifics from a prior game project and a prior
marketing-site project (real file paths, ticket prefixes, brand tokens,
collaborator names) from every agent/skill except `linear-dev-workflow`
(kept concrete on purpose) and the two skills that were already generic. See
[progress/2026-08-17-initial-genericization.md](../progress/2026-08-17-initial-genericization.md).

## Not yet done

See [open-questions.md](./open-questions.md) and [ideas/](../../vault/ideas/README.md).
