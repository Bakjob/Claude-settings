# Current status

Last updated: 2026-10-05

## What this repo is

A personal library of reusable Claude Code configuration, installable as the
Claude Code plugin `bakjob-surdeg` (marketplace `bakjob`). Its one live
skill, `bootstrap`, interviews the user about a new project and generates a
filled-in setup from the templates in `templates/`: `CLAUDE.md`, agents,
skills, a vault and `.claude/settings.json`. Pasting the repo into a project
still works through `BOOTSTRAP.md`.
See the root `README.md` for the full explanation and usage instructions.

## What's in the library right now

**`skills/bootstrap/`** — the plugin's only live skill: `SKILL.md` (flow),
`questions.md` (13 interview rounds), `generate.md` (what gets written per
answer). Added 2026-10-05.

Everything below lives under `templates/` (moved there 2026-10-05, see
[decision 0005](../decisions/0005-plugin-layout.md)).

**`CLAUDE-template.md`** — the root-level project instructions template.
Its tracker section is tracker-neutral since 2026-10-05.

**`vault/`** — skeleton for a generated project's vault (README, decisions
index, status files).

**`agents/`** (6, all Claude-Code subagents, all templated):
- `architect.md` — plans hard, multi-subsystem problems before code is written
- `config-value-auditor.md` — finds hardcoded values that should live in a central config file
- `seo-a11y-auditor.md` — audits a site against SEO/accessibility/performance targets
- `smoke-test-runner.md` — runs and interprets a project's smoke test
- `test-runner.md` — writes and runs automated tests, verifies the build
- `vault-scribe.md` — keeps a project's `vault/` up to date after real work

**`skills/`** (12):
- `git-checkpoint` — templated: checks whether now is a good commit/PR point
- `linear-dev-workflow` — **kept concrete** (real Linear MCP calls), only identifiers templated — see [decision 0003](../decisions/0003-linear-stays-concrete.md); installed when the tracker is Linear
- `github-issues-workflow` — concrete `gh` workflow with `in progress` / `in review` labels; installed when the tracker is GitHub Issues (added 2026-10-05, [decision 0006](../decisions/0006-tracker-chosen-at-bootstrap.md))
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

Turned the repo into a plugin with an interview-driven `bootstrap` skill, and
made the issue tracker a bootstrap choice instead of assuming Linear. See
[progress/2026-10-05-bootstrap-plugin.md](../progress/2026-10-05-bootstrap-plugin.md).
Planned and tracked as GitHub issues #2-#8. The repo now runs on GitHub
Issues and PRs itself ([decision 0007](../decisions/0007-github-flow-for-this-repo.md)).

## Not yet done

Issue #5: templates still use `[PLACEHOLDERS]` and get copied into each
project. The plan is for them to read project values from the project's own
files instead, so they can live in the plugin directly; that supersedes
[decision 0001](../decisions/0001-bracket-placeholder-convention.md).


See [open-questions.md](./open-questions.md) and [ideas/](../../vault/ideas/README.md).
