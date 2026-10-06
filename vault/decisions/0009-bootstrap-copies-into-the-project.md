# 0009: Bootstrap copies skills and agents into the project by default; plugin install is the option

**Status:** Superseded by [0010](./0010-plugin-install-is-the-default.md)

**Decision:** Bootstrap copies the accepted plugins' skills and agents (and
`bootstrap`, `feed`, `doctor`) into the project's `.claude/skills/` and
`.claude/agents/`, and writes no `enabledPlugins` or `extraKnownMarketplaces`.
Installing them as project-scope plugins stays as a choice in round 12. The
plugin split and the rule that skills read project values from the project's
files ([0008](./0008-per-project-plugins-read-project-files.md)) are
unchanged. The answer sheet records the install mode and the copied plugin
versions; `feed` refreshes copies from a fresh copy of the library, `doctor`
reports their age.

**Why:** The user does not want a project to depend on anything outside its
own folder: plugin installs live in `~/.claude/plugins/cache`, so the project
only works on a machine that has them, and (2026-10-06) pasted mode is what
they prefer. Because the skills hold no project values, copying them needs no
editing, so the original objection to copies in 0008 (filled-in copies
drifting) mostly goes away; only library improvements no longer arrive on
their own.

**Rules out:** Making plugin install the default again, or writing plugin
keys to a copy-mode project's `settings.json`. Also rules out putting
project values into the copies to "make them fit": they stay generic, or
the copy and the plugin drift apart.

**See also:** [0008](./0008-per-project-plugins-read-project-files.md),
[0005](./0005-plugin-layout.md).
