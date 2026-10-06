# Open questions

Unresolved things about how this library itself should work. Project ideas
(new agents/skills to build) go in `ideas/`, not here.

- **When does `bakjob-core` get too big?** It's ~1,100 always-on tokens
  with 11 components. If it grows much further, split it (e.g. a
  `bakjob-vault` plugin) so projects without a vault don't pay for it.
- **Has the bootstrap been run end to end?** Not yet (2026-10-05). The first
  real project bootstrapped with it will show which questions are missing,
  redundant or badly worded; fold that back into `questions.md`.
- **Should `smoke-test` (skill) and `smoke-test-runner` (agent) be merged
  into one artifact?** They currently overlap: the skill is the quick-check
  path, the agent is for when several checks need careful
  cross-referencing. Kept as two on purpose (matches the original), but
  worth revisiting once there's a second example of this pattern to compare
  against.
