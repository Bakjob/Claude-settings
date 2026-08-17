# 0001: Templates use bracket placeholders, not duplicated examples

**Status:** Active

**Decision:** Every agent and skill in this repo is written as a fillable
template using `[BRACKETED_PLACEHOLDER]` tokens for anything project-specific
(project name, hard-rules file, config file, tracker identifiers, commands),
the same convention `CLAUDE-template.md` already used. There is no separate
"filled-in example" copy kept alongside the template.

**Why:** The library started as a set of files pulled straight out of past
real projects (a game, a marketing site) with their actual specifics still
baked in — real ticket prefixes, real file paths, real tuning-variable names.
That made them read as documentation of those old projects, not as something
to copy into a new one. A single templated file that's obviously "fill this
in" is faster to reuse correctly than a clean template plus a worked example,
and it avoids the two ever drifting out of sync.

**Rules out:** Keeping a second "reference implementation" file per
agent/skill. If a worked example is ever wanted, it belongs in `ideas/` as a
scratch note, not as a permanent parallel file.

**See also:** [0002](./0002-generalize-not-delete.md)
