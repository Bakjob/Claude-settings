# Current status

Last updated: 2026-10-06

## What this repo is

A Claude Code marketplace, `bakjob`, published from
`Bakjob/Claude-settings` under the MIT license. See the root `README.md` for
how to install and use it.

## What's in it

Five plugins under `plugins/` ([decision 0008](../decisions/0008-per-project-plugins-read-project-files.md)):

- **`bakjob-surdeg`** (0.2.0, installed per user): `bootstrap`, with
  `SKILL.md` (flow), `questions.md` (13 interview rounds), `generate.md`
  (what each answer writes), `project-facts.md` (where the other plugins
  look things up), and its own templates `CLAUDE-template.md` and
  `vault-template/`.
- **`bakjob-core`** (0.1.0, per project): skills `git-checkpoint`,
  `vault-update`, `new-decision`, `smoke-test`, `test-all-branches`,
  `artifact-question-desk`; agents `architect`, `test-runner`, `vault-scribe`,
  `smoke-test-runner`, `config-value-auditor`.
- **`bakjob-github`** (0.1.0, per project): `github-issues-workflow`.
- **`bakjob-linear`** (0.1.0, per project): `linear-dev-workflow`, kept
  concrete ([decision 0003](../decisions/0003-linear-stays-concrete.md)).
- **`bakjob-web`** (0.1.0, per project): skills `design-taste-frontend`,
  `redesign-skill`, `content-page-family`, `static-site-deploy`; agent
  `seo-a11y-auditor`.

No skill outside bootstrap's own templates has placeholders; they read
project values from the project's files. This repo enables `bakjob-github`
for itself in `.claude/settings.json`.

## Just finished

Split into per-project plugins and removed placeholders from every shared
skill (#5, #13), on top of making the whole repo English (#11). See
[progress/2026-10-06-plugin-split.md](../progress/2026-10-06-plugin-split.md).

## Waiting on

- #3 and #4 are `in review`: bootstrap hasn't been run on a real project yet.

See [open-questions.md](./open-questions.md) and [ideas/](../ideas/README.md).
