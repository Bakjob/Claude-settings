# 2026-10-05: Bootstrap skill and plugin layout

## What changed

- The repo is a Claude Code plugin: `.claude-plugin/plugin.json`
  (`bakjob-surdeg`) and `.claude-plugin/marketplace.json` (`bakjob`).
  `claude plugin validate .` passes. The first name tried, `claude-settings`,
  was rejected: plugin names starting with `claude-` are reserved.
- All templates moved to `templates/` so the plugin only exposes
  `skills/bootstrap/` ([decision 0005](../decisions/0005-plugin-layout.md)).
- New `skills/bootstrap/`: `SKILL.md` (flow: pre-flight, interview, summary,
  generate, verify, report), `questions.md` (13 rounds covering project type,
  stack, languages, style, version control, commits/PRs, issue tracking,
  quality, docs, how Claude works, agents/skills), `generate.md` (what each
  answer writes).
- New `templates/vault/` skeleton and `BOOTSTRAP.md` for the pasted-in route.
- Issue tracker is a bootstrap choice
  ([decision 0006](../decisions/0006-tracker-chosen-at-bootstrap.md)): new
  `templates/skills/github-issues-workflow/`, tracker-neutral section in
  `CLAUDE-template.md`, a no-tracker todo-list variant in `generate.md`.
- README rewritten around the plugin and bootstrap.
- GitHub issues #2-#7 created for the plan, with #7 as the overview.
- Plugin renamed `bakjob-surdeg` (the user's pick: a sourdough starter for
  projects, with their GitHub name), and the repo got an MIT license so
  others can use it (#10). The repo got its own `CLAUDE.md` and a filled-in
  `.claude/skills/github-issues-workflow/`: GitHub issues and pull requests
  are the default here too
  ([decision 0007](../decisions/0007-github-flow-for-this-repo.md)), tracked
  as #8. Labels `in progress` and `in review` created on the repo.

## Why

The user pastes the whole library into each new project and lets Claude
generate from it, and wanted Claude to ask "lots of questions" about the
project's setup (version control, issue handling, language, and so on)
instead of guessing. They also wanted Linear not to be hardcoded.

## Not done

- Issue #5 (templates read project files instead of placeholders) is the
  bigger rework, left for its own PRs.
- The bootstrap hasn't been run on a real project yet.
