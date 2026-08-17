# 0003: `linear-dev-workflow` stays a real, usable Linear skill instead of a generic tracker template

**Status:** Active

**Decision:** Unlike most other skills in this repo, `linear-dev-workflow`
keeps its concrete Linear MCP tool calls (`mcp__linear__get_issue`,
`mcp__linear__save_issue`, `mcp__linear__save_comment`) rather than being
abstracted into a generic "issue tracker workflow" with the tool names
templated out. Only the per-project identifiers (workspace, team, project,
issue prefix, blocker list) are bracketed placeholders.

**Why:** Linear is the tracker actually used across real projects this
library gets copied into — genericizing away the tool names would make the
file need real rework (not just filling in blanks) every time it's copied,
which defeats the point of it being a template.

**Rules out:** Treating every skill the same way by default (see
[0002](./0002-generalize-not-delete.md)) — some things are worth keeping
concrete rather than generalized, when the concrete tool is the actual
constant across projects.

**See also:** [0002](./0002-generalize-not-delete.md)
