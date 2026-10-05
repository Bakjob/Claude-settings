# Open questions

Unresolved things about how this library itself should work. Project ideas
(new agents/skills to build) go in `ideas/`, not here.

- **Category subfolders for `skills/`?** Right now all 12 templates sit flat in `templates/skills/`.
  If the collection grows past ~20-25, flat may stop being scannable.
  Undecided whether to introduce subfolders (e.g. `skills/git/`,
  `skills/vault/`) or keep it flat and rely on the README's index instead.
- **Has the bootstrap been run end to end?** Not yet (2026-10-05). The first
  real project bootstrapped with it will show which questions are missing,
  redundant or badly worded; fold that back into `questions.md`.
- **Should `smoke-test` (skill) and `smoke-test-runner` (agent) be merged
  into one artifact?** They currently overlap: the skill is the quick-check
  path, the agent is for when several checks need careful
  cross-referencing. Kept as two on purpose (matches the original), but
  worth revisiting once there's a second example of this pattern to compare
  against.
