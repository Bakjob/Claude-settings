---
name: git-checkpoint
description: Check whether now is a reasonable point to commit, whether work is happening on a branch instead of main, and whether a PR is due. Use when asked "should we commit?" / "checkpoint" / "are we due for a PR?", or proactively suggest invoking this after finishing a coherent chunk of work, per the project's git workflow rules.
---

# Git checkpoint

[PROJECT_NAME]'s git workflow (see [HARD_RULES_FILE], "Git workflow") is: commit often at reasonable points, work on a branch, open a PR against `main` when a chunk of work is done or it's simply a good moment. This skill is the concrete check for "is now that moment."

## Steps

1. **Check the current branch.** `git branch --show-current`. If it's `main` and there are non-trivial uncommitted changes or the session has been making a series of edits, say so explicitly: work in progress belongs on a branch, not directly on `main`. Suggest a branch name (`feature/<slug>`, `fix/<slug>`, `chore/<slug>`) based on what's actually being done.

2. **Look at what's changed.** `git status` and `git diff` (staged and unstaged). Judge whether this is a reasonable commit boundary: a coherent piece of work finished (a bugfix, one pass's worth of a feature, a vault update), not a half-edited mid-thought state. Don't suggest committing broken or half-finished code just because time has passed.

3. **If it's a good checkpoint**, propose a commit message in this project's own style (see recent `git log` for tone: direct, states what and often why, no fluff) and ask before actually committing. Never commit automatically without confirmation from this skill; that's still the general rule.

4. **If the branch has a coherent, finished chunk of work and a remote to push to**, note that a PR against `main` may be due. Don't push or open the PR unprompted; flag it and let the user decide, since pushing affects shared state.

5. **If nothing meaningful has changed since the last commit**, say so plainly rather than manufacturing a checkpoint. Not every pause needs a commit.

This skill is a check, not an enforcer: it exists so whoever is working on this project gets reminded at sensible moments instead of either committing too eagerly mid-thought or letting a chunk of work pile up uncommitted on `main`.

---
*Template — fill in `[PROJECT_NAME]` and `[HARD_RULES_FILE]` for the target project.*
