---
name: smoke-test
description: Run the project's smoke test and report which checks pass or fail. Use after touching the subsystems the smoke test covers, or whenever asked to verify the project still runs without doing a full manual pass.
---

# Smoke test

Runs the project's smoke test and interprets the result, so the right flags and the right way to read the output don't have to be re-derived each time.

## Project facts

Everything project-specific lives in the `## Smoke test` section of `DOCS/running.md` (`DOCS` is the docs folder the project's `CLAUDE.md` names, usually `vault/`): the command, its run-length options or flags, any prep step, and one line per check with its pass criterion and the file it points at. If that section is missing or incomplete, ask the user once for what's missing, then add it there.

## Steps

1. **Check whether a prep step is needed first.** If new code was added since the last run that the smoke test wouldn't otherwise pick up (a new module, a new asset, a schema change), run the prep step `running.md` names. Skip it if nothing new was added; it costs real time for no benefit.

2. **Pick the run length and flags** from `running.md` based on what's being verified: a quick sanity check, or a longer run that exercises a specific subsystem.

3. **Run it.** A run that gives up before finishing (self-abort, timeout) is itself a signal that something stalled, not just a timeout to ignore.

4. **Interpret the output** against each check's own pass criterion from `running.md`. Don't assume a check printing at all means it passed. For anything beyond a quick eyeball, or if several checks need careful cross-referencing, delegate to the `smoke-test-runner` agent.

5. **Report** pass/fail per relevant check, with the actual printed numbers/output, not just "looks good." If something failed, name the file or subsystem it points at.
