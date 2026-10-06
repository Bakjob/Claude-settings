---
name: bootstrap
description: Set up Claude Code for a new (or existing) project by interviewing the user first (project type and stack, languages, version control, issue tracking, testing, docs/vault, how Claude should work), then generating CLAUDE.md, .claude/agents, .claude/skills, vault/ and .claude/settings.json from the library's templates. Use when the user says "bootstrap", "set up this project" (in any language), or pastes this library into a project folder and asks Claude to use it.
---

# Bootstrap a project

This skill turns the template library into a filled-in, project-specific
Claude Code setup. **The interview is the point.** The user wants to be asked,
round by round, about every choice the setup depends on, rather than have
Claude guess. Never skip a round silently; if the user says "use the defaults
for the rest" (or similar), take the recommended option for every remaining
question and say so in the summary.

## Where things are

- **This skill's folder** (`SKILL_DIR`): `${CLAUDE_SKILL_DIR}` when run as a
  plugin or project skill; when this file is being read by hand from a
  pasted copy, the folder this file is in. It holds `CLAUDE-template.md`,
  `vault-template/` and [project-facts.md](project-facts.md).
- **Library root** (`LIB`): the repo that holds `.claude-plugin/marketplace.json`
  and `plugins/`, the source the everyday skills and agents are copied from.
  In pasted mode it is `SKILL_DIR/../../../..`. When bootstrap runs from the
  installed plugin (the plugin cache holds only `bakjob-surdeg`), use
  `~/.claude/plugins/marketplaces/bakjob` if it exists (suggest
  `/plugin marketplace update bakjob` first, so the copy isn't stale);
  otherwise ask, then `git clone --depth 1
  https://github.com/Bakjob/Claude-settings` into a temp folder and delete it
  when done.
- **Target** (`TARGET`): the project being set up. Default to the current
  working directory (its git root if it has one). If the library itself was
  pasted into the target, `LIB` is a subfolder of `TARGET`; never treat
  `LIB/vault/` as the project's vault, it is the library's own history.

**How the pieces fit.** This plugin only bootstraps. The everyday skills and
agents live in the other plugins of the library (`bakjob-core`,
`bakjob-github`, `bakjob-linear`, `bakjob-web`, `bakjob-game`). They hold no
project values; they read them from the project's files as
[project-facts.md](project-facts.md) describes. So bootstrap does two
things: writes those files from the interview, and puts the right skills and
agents into the project. By default it copies them into `TARGET/.claude/`, so
the project depends on nothing outside its folder; it can install them as
plugins instead (round 12).

Confirm `TARGET` with the user in round 1 before writing anything.

## Steps

1. **Pre-flight.** Look at `TARGET`: is it empty, does it already have code,
   a `CLAUDE.md`, a `.claude/` folder, a git repo, a remote? If code exists,
   read enough of it (manifest files, top-level layout) to detect the stack,
   so the stack questions become confirmations instead of open questions.
   If `CLAUDE.md` or `.claude/` already exist, ask whether to merge into
   them, replace them, or stop. Never overwrite silently. If the project was
   bootstrapped before (a `progress/*-bootstrap.md` entry or a
   `setup.md` in the docs folder exists), suggest
   the `feed` skill instead: it changes only the parts the user picks.

2. **Interview.** Follow [questions.md](questions.md) round by round. Use the
   `AskUserQuestion` tool for multiple-choice rounds (at most 4 questions per
   call, 2-4 options each, the recommended option first with
   "(Recommended)" in its label). Ask free-text questions (name, pitch,
   tracker identifiers) as a plain message and wait for the answer. Ask in
   the language the user writes in. Skip questions whose answer is already
   known from pre-flight or earlier answers, but say what was inferred.

3. **Summary and confirmation.** Show every answer as one table, then the
   exact list of files that will be created or changed, plus any outward
   action (creating GitHub labels, creating a remote repo). Ask
   "Generate / Change something". Nothing is written before a yes.

4. **Generate.** Follow [generate.md](generate.md).

5. **Verify.** No `[BRACKETED_PLACEHOLDER]` may remain in any generated
   file (grep for `\[[A-Z][A-Z0-9_]+[^]]*\]`; skip the copied `bootstrap`
   skill, whose templates keep theirs), `.claude/settings.json` must parse as
   JSON, in copy mode must list no `bakjob` plugin, in plugin mode must list
   the enabled plugins, and every fact [project-facts.md](project-facts.md)
   names for the accepted plugins must be present. In copy mode, every skill
   and agent of the accepted plugins must exist under `.claude/`. Fix
   anything that fails before reporting.

6. **Report.** List what was written, what was deliberately left out and
   why (for example "smoke-test: add once a headless run exists"), and the
   next step. Mention `feed` for changing the setup later and `doctor` for
   checking it (in copy mode both are already in the project's
   `.claude/skills/`). If the library was pasted into `TARGET`, offer to delete that
   folder now that it has been used; delete only on an explicit yes.
