# 0008: Skills ship in per-project plugins and read project values from the project's files

**Status:** Active (0009 briefly made copying the default; plugin install is
the default again, see [0010](./0010-plugin-install-is-the-default.md))

**Decision:** The `bakjob` marketplace holds five plugins under `plugins/`.
`bakjob-surdeg` (bootstrap) is installed once per user. `bakjob-core`,
`bakjob-github`, `bakjob-linear` and `bakjob-web` are enabled per project
(project scope, in the project's `.claude/settings.json`), chosen by
bootstrap. Their skills and agents contain no `[BRACKETED_PLACEHOLDERS]`:
they read project values from `CLAUDE.md`, `DOCS/hard-rules.md`,
`DOCS/running.md` and friends, as
`plugins/bakjob-surdeg/skills/bootstrap/project-facts.md` lists, and ask once
and write the answer down when a value is missing. Only bootstrap's own
templates (`CLAUDE-template.md`, `vault-template/`) still use placeholders,
since bootstrap fills them once per project.

**Why:** Copied-in, filled-in skills never got library improvements, and a
single big plugin would have loaded every skill (web audits in a Godot game)
into every project and every session. The user picked the split
(2026-10-06, #5 and #13). Measured always-on cost after the split:
surdeg ~120 tokens, core ~1,100, web ~350.

**Rules out:** Putting project-specific values into a shared skill, and one
plugin that bundles everything. A new skill needing a project value adds it
to `project-facts.md` and bootstrap's `generate.md` instead.

**See also:** supersedes [0001](./0001-bracket-placeholder-convention.md)
and [0005](./0005-plugin-layout.md);
[0006](./0006-tracker-chosen-at-bootstrap.md) still holds (each tracker
plugin is concrete).
