---
name: linear-dev-workflow
description: Use at the start or end of implementing any task in a project tracked in Linear. Covers pulling issue context via the linear MCP tools, checking blockers, and updating status as work progresses.
---

# Working Linear issues for this project

The workspace, team, project and issue prefix (`PREFIX` below) are named in the "Issue tracker" section of the project's `CLAUDE.md`, together with the project's foundational blocker issues once they exist. If any of them is missing, ask the user once and add it there. If no `mcp__linear__*` tools are available, tell the user to connect Linear: `claude mcp add --transport http linear https://mcp.linear.app/mcp`, then `/mcp` to log in.

## Before starting an issue

0. **If no issue exists yet** for the chunk of work about to start, create one (`mcp__linear__create_issue`) at that moment, not after the fact — per the issue-tracker rules in `CLAUDE.md`, every new feature, bug, and improvement goes through Linear, not just bugs.
1. Pull the full issue with `mcp__linear__get_issue` (list views truncate descriptions) — don't work from a stale summary.
2. Check whether it depends on one of the foundational blockers `CLAUDE.md` lists (e.g. infra setup, content/asset collection, design sign-off, with their own issue IDs). If the issue you're picking up assumes one of these is done, verify it actually is before starting — don't invent content or infrastructure to unblock yourself silently.
3. Move the issue to "In Progress" **and set its assignee** in the same `mcp__linear__save_issue` call (`id` + `state` + `assignee`) so Linear reflects real status and real ownership, not just status. **Never overwrite an assignee that's already set** — that person is on it; an unassigned issue is the actual failure, not one assigned to someone else.

## While working

- If you discover missing scope while implementing, create a new issue rather than silently expanding the current one's scope. Attach it to the same team/project.
- Reference related issue IDs in descriptions (e.g. "see PREFIX-24") rather than duplicating their content.
- **Name the issue ID in commit messages** for anything that fixes or works toward it (e.g. `PREFIX-30: fix ...`), so it's clear from git log alone which commits are trying to fix what.
- **PR descriptions get no "Test plan" checklist.** Whatever manual verification is still needed goes on the Linear issue instead (`mcp__linear__save_comment`), as real checkboxes, not prose in the PR body — see step 1 below.
- If Linear's GitHub integration auto-transitions issues off PR state, **put the issue ID in the PR title itself**, not only the body or commit messages — that's usually what the integration matches on. Still worth a manual check after a merge, the automation can silently fail to match.
- **Sync Linear the moment anything merges, whoever merged it.** When a `git pull`, `gh pr list`, or similar surfaces a PR that merged without you, check the matching issue then, not only when asked.

## Finishing an issue

"Done" means a human verified it, not that it merged — see the issue-tracker section of `CLAUDE.md` for the full In Review loop. In short:

1. Once the PR merges, move state to **"In Review"** (not Done) via `mcp__linear__save_issue`, with a comment (`mcp__linear__save_comment`) listing what still needs a human look as a real checklist — visual polish, a real device, stakeholder sign-off, anything an automated check can't prove. If the issue was fully provable by the project's own automated checks (lint/type/test suite) and nothing needs human eyes, skip straight to Done instead.
2. Also leave a comment if the implementation deviated from the issue description in any way worth recording — future issues will assume the build matches the written spec.
3. When the user later confirms testing (a plain "works"/"tested" statement), move the issue from In Review to Done in that same turn. If they report something broken or incomplete instead, move it back to In Progress and repeat.

