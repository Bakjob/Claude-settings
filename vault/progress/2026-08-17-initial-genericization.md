# 2026-08-17: Initial genericization pass + vault setup

The library existed as a set of files pulled from two past real projects (a
Godot game and a marketing website) with their specifics still baked in:
real file paths, real ticket prefixes, real tuning-variable and brand-token
names, and one former collaborator's name in `test-all-branches`. None of it
was safely copy-pasteable into an unrelated project without a manual find/
replace pass first.

## What changed

- Rewrote all 6 agents and 8 of the 10 skills as bracket-placeholder
  templates, following the convention `CLAUDE-template.md` already used.
  `design-taste-frontend` and `redesign-skill` needed no changes — they were
  already fully generic.
- Renamed two skills to the general pattern they actually encode:
  `case-study-page` → `content-page-family`, `loopia-deploy` →
  `static-site-deploy`. Renamed `agents/balance-auditor.md` →
  `agents/config-value-auditor.md` for the same reason.
- Kept `linear-dev-workflow` concrete on purpose rather than templating away
  the Linear MCP tool calls — see
  [decision 0003](../decisions/0003-linear-stays-concrete.md).
- Replaced the former collaborator's name in `test-all-branches` with a
  generic "the user."
- Added the root `README.md` explaining what the repo is, how to copy
  pieces of it into a new project, and documenting the vault.
- Built this `vault/` (this repo's own second brain), seeded with
  `decisions/0001`–`0004` recording the calls made in this pass.

## Why this shape

See [decisions 0001–0004](../decisions/README.md) for the individual calls;
the short version is: template, don't duplicate; generalize narrow skills
instead of deleting them; keep Linear concrete since it's a real constant
across projects this gets copied into; and keep this vault scoped to the
library itself rather than doubling as an example vault.

## What's still open

See [status/open-questions.md](../status/open-questions.md) and
[ideas/](../ideas/README.md).
