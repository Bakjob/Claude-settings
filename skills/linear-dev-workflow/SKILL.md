---
name: linear-dev-workflow
description: Use at the start or end of implementing any [PROJECT_NAME] task tracked in Linear (team "[TRACKER_TEAM]", project "[TRACKER_PROJECT]", issue prefix [PREFIX]). Covers pulling issue context via the linear MCP tools, checking blockers, and updating status as work progresses.
---

# Working Linear issues for this project

Workspace: `linear.app/[WORKSPACE]`. Team: **[TRACKER_TEAM]**. Project: **[TRACKER_PROJECT]**. All issues use the `[PREFIX]-*` prefix.

## Before starting an issue

1. Pull the full issue with `mcp__linear__get_issue` (list views truncate descriptions) — don't work from a stale summary.
2. Check whether it depends on one of this project's foundational blockers (list them here once they exist, e.g. infra setup, content/asset collection, design sign-off — with their own issue IDs). If the issue you're picking up assumes one of these is done, verify it actually is before starting — don't invent content or infrastructure to unblock yourself silently.
3. Move the issue to "In Progress" **and set its assignee** in the same `mcp__linear__save_issue` call (`id` + `state` + `assignee`) so Linear reflects real status and real ownership, not just status.

## While working

- If you discover missing scope while implementing, create a new issue rather than silently expanding the current one's scope. Attach it to the same team/project.
- Reference related issue IDs in descriptions (e.g. "see [PREFIX]-24") rather than duplicating their content.

## Finishing an issue

"Done" means a human verified it, not that it merged — see [HARD_RULES_FILE]'s "Tracking" section for the full In Review loop. In short:

1. Once the PR merges, move state to **"In Review"** (not Done) via `mcp__linear__save_issue`, with a comment (`mcp__linear__save_comment`) listing what still needs a human look as a real checklist — visual polish, a real device, stakeholder sign-off, anything an automated check can't prove. If the issue was fully provable by the project's own automated checks (lint/type/test suite) and nothing needs human eyes, skip straight to Done instead.
2. Also leave a comment if the implementation deviated from the issue description in any way worth recording — future issues will assume the build matches the written spec.
3. When the user later confirms testing (a plain "works"/"tested" statement), move the issue from In Review to Done in that same turn. If they report something broken or incomplete instead, move it back to In Progress and repeat.

---
*Template — fill in `[PROJECT_NAME]`, `[WORKSPACE]`, `[TRACKER_TEAM]`, `[TRACKER_PROJECT]`, `[PREFIX]`, and `[HARD_RULES_FILE]` for the target project. Keep this one as a real, usable Linear workflow (not just a generic placeholder) — swap the identifiers per project rather than genericizing away the Linear MCP tool calls.*
