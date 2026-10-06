---
name: bootstrap
description: Set up Claude Code for a new (or existing) project by interviewing the user first: project type and stack, languages, version control, issue tracking, testing, docs/vault, how Claude should work, then generating CLAUDE.md, .claude/agents, .claude/skills, vault/ and .claude/settings.json from the library's templates. Use when the user says "bootstrap", "set up this project", "sätt upp projektet", or pastes this library into a project folder and asks Claude to use it.
---

# Bootstrap a project

This skill turns the template library into a filled-in, project-specific
Claude Code setup. **The interview is the point.** The user wants to be asked,
round by round, about every choice the setup depends on, rather than have
Claude guess. Never skip a round silently; if the user says "use the defaults
for the rest" (or similar), take the recommended option for every remaining
question and say so in the summary.

## Where things are

- **Library root** (`LIB`): two levels up from this file. In plugin mode that
  is `${CLAUDE_SKILL_DIR}/../..`; when the library was pasted into a project
  folder and this file is being read by hand, resolve it relative to this
  file's own path.
- **Templates:** `LIB/templates/CLAUDE-template.md`, `LIB/templates/agents/`,
  `LIB/templates/skills/`, `LIB/templates/vault/`.
- **Target** (`TARGET`): the project being set up. Default to the current
  working directory (its git root if it has one). If the library itself was
  pasted into the target, `LIB` is a subfolder of `TARGET`; never treat
  `LIB/vault/` as the project's vault, it is the library's own history.

Confirm `TARGET` with the user in round 1 before writing anything.

## Steps

1. **Pre-flight.** Look at `TARGET`: is it empty, does it already have code,
   a `CLAUDE.md`, a `.claude/` folder, a git repo, a remote? If code exists,
   read enough of it (manifest files, top-level layout) to detect the stack,
   so the stack questions become confirmations instead of open questions.
   If `CLAUDE.md` or `.claude/` already exist, ask whether to merge into
   them, replace them, or stop. Never overwrite silently.

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
   "Generera / Ändra något". Nothing is written before a yes.

4. **Generate.** Follow [generate.md](generate.md).

5. **Verify.** No `[BRACKETED_PLACEHOLDER]` may remain in any generated
   file (grep for `\[[A-Z][A-Z0-9_]+[^]]*\]`), `.claude/settings.json` must
   parse as JSON, every copied skill must have a valid frontmatter. Fix
   anything that fails before reporting.

6. **Report.** List what was written, what was deliberately left out and
   why (for example "smoke-test: add once a headless run exists"), and the
   next step. If the library was pasted into `TARGET`, offer to delete that
   folder now that it has been used; delete only on an explicit yes.
