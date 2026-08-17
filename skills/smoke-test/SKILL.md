---
name: smoke-test
description: Run [PROJECT_NAME]'s smoke test and report which checks pass or fail. Use after touching [KEY SUBSYSTEMS], or whenever asked to verify the project still runs without doing a full manual pass.
---

# Smoke test

Runs [PROJECT_NAME]'s smoke test and interprets the result. See [RUNNING_FILE] for the canonical command; this skill exists so the right flags and the right way to read the output don't have to be re-derived each time.

## Steps

1. **Check whether a build/setup step is needed first.** If new code was added since the last run that the smoke test wouldn't otherwise pick up (a new module, a new asset, a schema change), run whatever prep step the project needs first. Skip this if nothing new was added; it costs real time for no benefit.

2. **Pick the run length and flags** based on what's being verified. *(Document this project's actual options here: quick sanity check vs. a longer/more thorough run, any speed multiplier, any flag that exercises a specific subsystem.)*

3. **Run it:**
   ```
   [SMOKE_TEST_COMMAND] [flags]
   ```
   Note any self-abort/timeout behavior: a run that gives up before finishing is itself a signal something stalled, not just a timeout to ignore.

4. **Interpret the output.** Each check should state its own pass criterion. For anything beyond a quick eyeball, or if several checks need careful cross-referencing against what they're supposed to prove, delegate to the `smoke-test-runner` agent if one exists for this project. For a fast single check, read the check's own stated criterion directly.

5. **Report** pass/fail per relevant check, with the actual printed numbers/output, not just "looks good." If something failed, name the file or subsystem it points at.

---
*Template — fill in `[PROJECT_NAME]`, `[RUNNING_FILE]`, `[SMOKE_TEST_COMMAND]`, and the checks that actually exist for the target project.*
