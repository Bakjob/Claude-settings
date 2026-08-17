# Open questions

Unresolved things about how this library itself should work. Project ideas
(new agents/skills to build) go in `ideas/`, not here.

- **Category subfolders for `skills/`?** Right now all 10 skills sit flat.
  If the collection grows past ~20-25, flat may stop being scannable.
  Undecided whether to introduce subfolders (e.g. `skills/git/`,
  `skills/vault/`) or keep it flat and rely on the README's index instead.
- **A `.claude/settings.json` / hooks template?** The library currently only
  covers agents, skills, and `CLAUDE.md`. No opinion yet on whether a
  generic starter `settings.json` (permissions, hooks) belongs here too, or
  whether that's too project-specific to templatize usefully.
- **Should `smoke-test` (skill) and `smoke-test-runner` (agent) be merged
  into one artifact?** They currently overlap: the skill is the quick-check
  path, the agent is for when several checks need careful
  cross-referencing. Kept as two on purpose (matches the original), but
  worth revisiting once there's a second example of this pattern to compare
  against.
