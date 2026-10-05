---
name: github-issues-workflow
description: Use at the start or end of implementing any Claude-settings task tracked in GitHub Issues (repo Bakjob/Claude-settings). Covers creating and reading issues with the gh CLI, claiming them, the "in progress" / "in review" labels, and closing an issue only once it is verified.
---

# Working GitHub issues for this project

Repo: **Bakjob/Claude-settings**. Issues are tracked with GitHub Issues through the `gh` CLI.
State lives in two labels, since GitHub issues are only open or closed:
`in progress` (someone is working on it) and `in review` (merged, waiting for
a human to verify). Closed means Done.

## Before starting an issue

0. **If no issue exists yet** for the chunk of work about to start, create one
   at that moment, not after the fact (per CLAUDE.md's issue-tracker
   rules every feature, bug and improvement goes through the tracker):
   ```
   gh issue create -R Bakjob/Claude-settings --title "..." --body-file <file> --label enhancement --assignee @me
   ```
   Use `bug` / `enhancement` / `documentation` labels for the kind of work.
1. Read the full issue with its comments, not a list summary:
   `gh issue view N -R Bakjob/Claude-settings --comments`
2. Check who is on it: `gh issue view N -R Bakjob/Claude-settings --json assignees,labels`.
   **Never take over an issue someone else is assigned to**; that person is on
   it. An unassigned issue is the actual failure.
3. Claim it and mark it started in one command, before the first edit:
   ```
   gh issue edit N -R Bakjob/Claude-settings --add-assignee @me --add-label "in progress"
   ```

## While working

- Missing scope discovered mid-work gets its own issue, not a silent expansion
  of the current one. Link it: "see #24".
- **Name the issue in commit messages** (`#30: fix ...`) so git log alone
  shows which commits work toward what.
- **Put the issue number in the PR title** too, e.g. `🐛 #30 Fix save corruption`.
- **Use `Refs #N` in the PR body, not `Closes #N`.** A merge must not close
  the issue: Done means verified, not merged. (Projects that chose the simple
  flow use `Closes #N` instead, and skip the In Review steps below.)
- **PR descriptions get no test-plan checklist.** Manual checks go on the
  issue as a comment with real checkboxes.
- **Sync the moment anything merges, whoever merged it.** When `git pull` or
  `gh pr list --state merged` shows a PR you didn't see merge, update its
  issue then.

## Finishing an issue

1. When the PR merges, move the issue to In Review and attach what still
   needs a human, as checkboxes:
   ```
   gh issue edit N -R Bakjob/Claude-settings --remove-label "in progress" --add-label "in review"
   gh issue comment N -R Bakjob/Claude-settings --body-file <checklist.md>
   ```
   If the project's automated checks prove the whole issue and nothing needs
   human eyes, skip In Review and close it directly (step 3).
2. Comment if the implementation deviated from the issue description; later
   issues will assume the build matches the written spec.
3. When the user says it works or is tested, close it in the same turn:
   ```
   gh issue edit N -R Bakjob/Claude-settings --remove-label "in review"
   gh issue close N -R Bakjob/Claude-settings --reason completed
   ```
   If they report it broken or incomplete, swap `in review` back to
   `in progress` and keep going. That can happen more than once.

