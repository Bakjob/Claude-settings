# 0010: Bootstrap enables plugins by default; copying into the project is the option

**Status:** Active

**Decision:** Bootstrap installs the accepted plugins at project scope
(`enabledPlugins` and `extraKnownMarketplaces` in `.claude/settings.json`) by
default. Copying their skills and agents into `.claude/` stays as a choice in
round 12, and is the only mode when the library was pasted into the project.
`feed` and `doctor` keep handling both modes. Everything else from
[0009](./0009-bootstrap-copies-into-the-project.md) (the answer sheet's
install mode and versions, `feed` refreshing copies) stays.

**Why:** The user reversed 0009 on 2026-10-07: one shared copy that Claude
Code updates, so library improvements reach projects without a manual
refresh, outweighs keeping the project independent of `~/.claude/plugins`.

**Rules out:** Making copying the default again without a new decision, and
dropping copy mode altogether (it is what pasted mode relies on).

**See also:** [0008](./0008-per-project-plugins-read-project-files.md),
[0009](./0009-bootstrap-copies-into-the-project.md).
