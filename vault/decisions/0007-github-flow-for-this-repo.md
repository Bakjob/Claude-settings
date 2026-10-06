# 0007: This repo itself uses GitHub Issues and pull requests

**Status:** Active

**Decision:** Every change to the library gets a GitHub issue on
`Bakjob/Claude-settings` and lands through a pull request from a branch,
never a direct commit to `main`. Claude may commit, push and open the PR
once a change is verified; merging is the user's. The workflow is in the
root `CLAUDE.md`, the commands in `.claude/skills/github-issues-workflow/`.

**Why:** The user wanted the library to work the way the projects it sets up
do, with GitHub and issue handling as the default and pull requests as the
way new code lands.

**Rules out:** Direct commits to `main`, even for small docs changes, and
tracking this repo's work only in `vault/ideas/` without an issue.

**See also:** [0006](./0006-tracker-chosen-at-bootstrap.md)
