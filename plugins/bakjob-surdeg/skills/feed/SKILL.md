---
name: feed
description: Change part of an existing project's Claude setup without re-running bootstrap - switch issue tracker, change the git workflow or who merges, add a deploy or release target, turn game or web tools on or off, refresh the copied skills, change languages, tests, vault or permissions. Re-asks only the interview rounds the user picks, with today's answers as defaults, and changes only what those answers touch. Use when the user wants to change how the project is set up, or when doctor points at a structural change.
---

# Feed

A sourdough starter gets fed to keep it alive; a project's setup gets fed
when the project changes. Same interview as bootstrap, but only the parts
the user picks, starting from what's true today.

Uses bootstrap's own files: [questions.md](../bootstrap/questions.md) for
the rounds, [generate.md](../bootstrap/generate.md) for what each answer
writes, [project-facts.md](../bootstrap/project-facts.md) for where facts
live.

## Steps

1. **Read today's setup.** The answer sheet in the vault's
   `progress/*-bootstrap.md` entry (and any later `*-feed.md` entries, which
   override it), or `DOCS/setup.md` in a light vault, `CLAUDE.md`, `.claude/settings.json` and the fact files.
   Without an answer sheet (a project set up by hand), reconstruct the
   answers from the files, show them, and have the user confirm them first.
   Where the files and the sheet disagree, the files win: they're what the
   plugins actually read.

2. **Ask what to change.** One `AskUserQuestion` call with two multi-select
   questions:
   - [Version control and commits · Issue tracking · Languages and style · Quality and tests]
   - [Docs and vault · How Claude works (push back, permissions) · Deploy and release · Plugins and skills (add, remove, refresh)]
   If the user already said what they want ("switch to Linear"), skip this
   and go to the matching rounds.

3. **Re-run those rounds** from `questions.md`, with the current answer
   first and labelled "(current)" instead of the usual recommendation, so
   keeping things is one click. Follow-up questions apply as in bootstrap
   (e.g. Linear identifiers after choosing Linear).

4. **Show the change before making it.** Per file: the section, before →
   after. Plus every outward action: skills and agents added, removed or refreshed (or plugins turned on or off), labels
   created, issues created by a migration. Ask
   [Apply · Change something · Cancel].

5. **Apply** by running only the steps of `generate.md` the changed answers
   touch. Edit the affected sections in place and leave everything else in
   each file as it is, including the user's own additions. Skills and agents,
   by the **Install mode** row of the answer sheet:
   - **Copy:** needs the library as `LIB` (resolve it as bootstrap's
     [SKILL.md](../bootstrap/SKILL.md) says: the marketplace clone, else a
     shallow clone into a temp folder, asked first and deleted after).
     *Add* a plugin: copy its `skills/*/` and `agents/*.md` into
     `.claude/` as in generate.md step 4. *Remove* one: delete only the
     skills and agents of that plugin, and show any that were edited by hand
     first. *Refresh*: `diff -r` each copied skill and agent against `LIB`,
     show which files differ, and overwrite only what the user accepts; a
     file the user changed on purpose gets its own question. Do `feed`,
     `doctor` and `bootstrap` last, since they are what's running. Then
     update the plugin versions in the answer sheet.
   - **Plugins:**
     `claude plugin install <plugin>@bakjob --scope project` or
     `claude plugin uninstall <plugin>@bakjob --scope project`, keeping
     `extraKnownMarketplaces` in place while any `bakjob` plugin is on.
   - Switching between the two modes: install or copy first, then remove the
     other kind (the copies, or the `enabledPlugins` and
     `extraKnownMarketplaces` keys), so nothing shadows anything.

6. **Migrate what the change leaves behind**, each one offered, never
   automatic:
   - No tracker → GitHub Issues or Linear: turn the open lines in
     `DOCS/status/todo.md` into issues, then remove the file and its
     `CLAUDE.md` section.
   - Tracker → another tracker: list the open issues and offer to recreate
     them with a link back; the old tracker stays read-only.
   - Straight to main → branches and PRs: if there's uncommitted work on
     `main`, move it to a branch first.
   - Vault None → Light/Full: create it from `vault-template/` and seed
     `current-status.md` from the git history.

7. **Record it.** A `progress/YYYY-MM-DD-feed.md` entry with the changed
   answers as a before → after table (this is the answer sheet's update); in
   a light vault, update `DOCS/setup.md` in place instead.
   If a change reverses a settled decision in `decisions/`, say which one
   and ask "Are you sure?" before step 5, then write a decision that
   supersedes it.

8. **Check** with the `doctor` skill's checks for the areas touched, and
   remind the user to restart Claude Code if plugins changed.
