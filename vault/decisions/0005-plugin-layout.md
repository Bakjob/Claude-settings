# 0005: The repo is a Claude Code plugin; templates live in `templates/`, outside the plugin's component folders

**Status:** Active

**Decision:** The repo ships `.claude-plugin/plugin.json` (plugin
`bakjob-kickstart`) and `.claude-plugin/marketplace.json` (marketplace
`bakjob`), so it installs with `/plugin marketplace add Bakjob/Claude-settings`.
The plugin's only live component is `skills/bootstrap/`. Every template
(`CLAUDE-template.md`, agents, skills, the vault skeleton) moved to
`templates/`. Pasting the repo into a project still works through
`BOOTSTRAP.md`.

**Why:** The user pasted the whole library into each new project and let
Claude generate from it. That meant a stale copy per project and this repo's
own `vault/` landing next to the new project's files. A plugin installs once
and updates everywhere. A plugin loads everything under `skills/` and
`agents/` automatically (custom paths in `plugin.json` only add to those,
they don't replace them), so the unfilled templates had to move out, or they
would show up as live skills full of `[PLACEHOLDERS]` in every project.

**Rules out:** Putting templates back under the root `skills/` or `agents/`
while they still contain placeholders. A template moves there only once it
reads its project-specific values from the project's own files (issue #5).
Also rules out plugin names starting with `claude-`, which Claude Code
reserves.

**See also:** [0004](./0004-vault-scoped-to-this-repo.md),
[0006](./0006-tracker-chosen-at-bootstrap.md)
