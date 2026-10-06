---
name: playtest
description: Verify a gameplay change by splitting its checks into what a headless, deterministic run can prove (rules, numbers, invariants, no crashes over N simulated minutes) and what only a human playing can judge (feel, juice, difficulty, readability), running the first and putting the second on the issue as a checklist. Use after changing gameplay, controls, balance or game feel, and before moving a gameplay issue to review.
---

# Playtest

"Done means verified" is hard for games: much of what matters is how it
feels. This skill keeps Claude honest about which part it can prove and
hands the rest to the user as concrete things to try.

## Project facts

The headless run command, its flags and its checks are in the `## Smoke
test` section of `DOCS/running.md` (`DOCS` is the docs folder the project's
`CLAUDE.md` names, usually `vault/`), and the design intent in
`DOCS/game-design/` when it exists. If the project has no headless run yet,
say so, check what can be checked by unit tests, and suggest adding one
(see "Building a headless run").

## Steps

1. **Write down what the change is supposed to do**, from the issue and the
   game design notes: the rule, the number, the feeling it's going for.

2. **Split it into two lists:**
   - **Provable:** rules and numbers ("a perfect parry refunds 1 stamina"),
     invariants ("health never goes below 0", "the player can't leave the
     level bounds"), stability ("no errors or NaN positions over 10 simulated
     minutes"), regressions in other systems.
   - **Needs a human:** does the jump feel floaty, is the hit readable, is
     the new enemy fair, does the screen shake help or annoy, is the
     tutorial clear.

3. **Prove the first list:** unit tests for rules and numbers; the headless
   run for invariants and stability, with a fixed seed so a failure
   reproduces. Report actual numbers, not "passed".

4. **Hand over the second list** as checkboxes on the issue (through the
   project's tracker skill), each one concrete enough to act on: what to do
   in the game, and what to pay attention to. "Dash into the wall at full
   speed 5 times: does the stop feel too abrupt?" not "check the dash".

5. **Report** both lists: what was proven and how, what is waiting for the
   user.

## Building a headless run

When a project doesn't have one, propose this shape (in the engine's own
terms, see `engine-conventions`):

- Starts the game without a window, with a fixed timestep and a seeded RNG,
  so two runs with the same seed are identical.
- Feeds scripted or random-but-seeded input (a simple bot is enough: move,
  jump, attack at random).
- Runs for a set number of simulated seconds as fast as possible.
- Asserts invariants every tick and prints one line per check with its pass
  criterion, so `smoke-test` and `smoke-test-runner` can read it.
- Exits non-zero on failure.

Write the command and its checks into `running.md`'s `## Smoke test`
section once it exists.
