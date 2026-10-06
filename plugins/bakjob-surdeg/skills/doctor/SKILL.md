---
name: doctor
description: Health check for a project's Claude Code setup - CLAUDE.md, the bakjob skills and agents (copied or installed), the project facts those plugins read, vault freshness, tracker hygiene and git basics - reported as a table, with fixes applied only after the user approves them. Use when the user asks to check, diagnose or "doctor" the project's setup, after refreshing the bakjob skills, when a bakjob skill keeps asking for the same facts, or in a project that was set up by hand.
---

# Doctor

Checks what the bakjob plugins depend on and reports what's missing, stale
or inconsistent. It reads; it changes nothing until the user approves a
fix. Works in any project, bootstrapped or not.

## What it checks against

[project-facts.md](../bootstrap/project-facts.md) is the contract: which
file holds which fact, per plugin. The plugin list and what each one needs
are in round 12 of [questions.md](../bootstrap/questions.md).

## Steps

Run the checks, collecting each result as **OK**, **Warning** (works, but
something is stale or missing that a skill will ask for later) or
**Missing** (a skill can't do its job). Don't stop at the first problem.

1. **CLAUDE.md.** Exists at the repo root; first heading is the project
   name with a pitch under it; names the docs folder (`DOCS`); has an issue
   tracker section (or a todo variant) and a git workflow section; no
   `[BRACKETED_PLACEHOLDER]` left (grep `\[[A-Z][A-Z0-9_]+[^]]*\]`).

2. **Skills and agents.** First find the install mode: the **Install mode**
   row of the answer sheet (`progress/*-bootstrap.md` or `DOCS/setup.md`),
   confirmed against `.claude/`.
   - **Copy** (no `bakjob` keys in `.claude/settings.json`): which bakjob
     skills and agents are in `.claude/skills/` and `.claude/agents/`
     (names from round 12 of [questions.md](../bootstrap/questions.md)), as
     whole plugins' worth; skills or agents that fit the project but are
     missing (a game without `engine-conventions`, GitHub Issues in
     `CLAUDE.md` without `github-issues-workflow`); ones that don't fit (web
     tools in a Godot project); copies edited by hand (offer to keep or
     refresh them, never overwrite silently). Copies don't update on their
     own: if the answer sheet's plugin versions are older than the
     library's, say so and point to `feed`. Doctor itself downloads nothing.
   - **Plugins** (`.claude/settings.json`): which `bakjob-*` plugins are in
     `enabledPlugins`, and whether `extraKnownMarketplaces` has `bakjob`
     (without it, collaborators aren't offered the plugins); plugins that
     fit but are off, or don't fit; skills or agents in `.claude/` with the
     same name as a plugin's (left over from copying): they shadow the plugin
     and never update. `claude plugin list` shows the installed versions; if
     the marketplace hasn't been refreshed in a while, suggest
     `/plugin marketplace update bakjob`.
   - In either mode, say once that plugin mode keeps files in
     `~/.claude/plugins`, outside the project, and copy mode doesn't.

3. **Facts.** For each enabled plugin, every fact `project-facts.md` lists:
   the `DOCS/running.md` sections, `DOCS/hard-rules.md` (and its config
   file line if `config-value-auditor` will be used), `quality-targets.md`
   for web and game projects, the tracker identifiers in `CLAUDE.md`. A
   section saying "not set up yet" is a Warning, not Missing, if the project
   has no code for it yet. Check that commands still match reality: the
   scripts in `package.json`, the engine version in the project file.

4. **Vault.** `status/current-status.md`'s "Last updated" date against the
   latest commits (`git log -1 --format=%cs`); merged PRs or a stretch of
   commits since the newest `progress/` entry; the decisions index matching
   the files in `decisions/`; open questions that a later decision already
   answered.

5. **Tracker.** For GitHub Issues: the `in progress` and `in review` labels
   exist (`gh label list`); open issues labelled `in progress` with no
   commits or comments for two weeks; PRs merged since the last check whose
   issue is still `in progress`. For Linear, the same through the Linear
   MCP tools if they're connected. For a todo list: ticked lines that no
   progress entry records.

6. **Git.** Uncommitted work sitting on `main` when the workflow says
   branches; `.gitignore` covering the stack's generated folders, `.env*`,
   and `.visual-check/` for web; Git LFS set up if the repo has large binary
   assets (`git lfs ls-files`, `.gitattributes`).

## Report

One table: area, check, result, what to do. Missing first, then Warnings,
then a single line for everything OK. Then offer the fixes as a numbered
list and ask which to apply. Fix rules:

- A missing fact is asked for once and written into the file
  `project-facts.md` names, same as any bakjob skill would.
- Adding or removing a plugin's skills is a `feed` job (it knows both
  install modes); point there instead of doing it here.
- In plugin mode, a leftover copied skill is deleted only after showing
  whether it differs from the plugin's version.
- Anything that changes the setup's structure (switching tracker, changing
  the git workflow) belongs to `feed`; point there instead of doing it here.

End with what was fixed and what's still open.
