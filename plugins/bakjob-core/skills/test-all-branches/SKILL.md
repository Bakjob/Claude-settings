---
name: test-all-branches
description: Rebuild a disposable local branch that combines every currently open PR, so whoever is testing can try everything in flight at once without merging anything to main first. Use whenever asked to "test all the branches" / "get everything live to test" / after finishing several PRs in one session, or whenever new PRs have landed since the branch was last built.
---

# Test all branches

The project's git workflow keeps changes on separate, small PRs (see the "Git workflow" section of its `CLAUDE.md`), but a human testing the project wants everything new running at once, not one PR's worth of change at a time. This skill builds that: a local-only branch, `local-test-all`, that is every open PR merged together on top of `main`, rebuilt fresh each time this runs.

**This branch is disposable and never pushed.** It exists purely so there is something to run/test from. The real merges into `main` still happen one PR at a time, on purpose, after whoever tested it says so.

## Steps

1. **Protect whatever is currently checked out.** `git status --short`. If there are uncommitted changes on the current branch, stash them (`git stash push -u`) or say so and stop, per the global rule: never discard uncommitted work silently.

2. **Fetch and rebuild the scratch branch from scratch, every time:**
   ```
   git fetch origin --quiet
   git checkout main
   git branch -D local-test-all 2>/dev/null
   git checkout -b local-test-all origin/main
   ```
   Deleting and recreating rather than reusing is deliberate: a stale `local-test-all` that only merged in NEW commits since last time could silently keep a PR that was closed/superseded, or miss that a PR's branch was force-pushed. Starting from `origin/main` fresh every run is the only way the result is always exactly "main plus every PR open right now."

3. **List every open PR and merge each head branch in turn:**
   ```
   gh pr list --json number,headRefName
   ```
   then for each, `git merge --no-edit origin/<headRefName>`.

4. **On a conflict, read it before resolving it.** Most conflicts here are incidental: two branches touched the same nearby line for unrelated reasons. Resolve those by keeping both branches' actual intent, not by picking a side blind. If a conflict looks like two PRs took genuinely different approaches to the same problem, STOP and say so explicitly instead of guessing which one should win; that is a real decision, not a merge mechanic.

5. **Rebuild/re-import and smoke-test the result**, since combining several branches can surface a new dependency needing a fresh setup step, or an interaction none of the individual PRs hit alone:
   use the commands in the `## Setup` and `## Smoke test` sections of `DOCS/running.md` (the docs folder `CLAUDE.md` names, usually `vault/`). If there is no smoke test yet, run the build and test commands from the same file instead, and say so in the report.

6. **Report**, plainly: which PRs are merged into this run, any conflicts found and how they were resolved, and the smoke test result. Leave the working tree checked out on `local-test-all`, ready to use.

If a PR gets merged into `main` for real between test sessions, this skill still works unchanged next time: rebuilding from `origin/main` fresh means an already-merged PR is simply already there, and `gh pr list` no longer includes it.
