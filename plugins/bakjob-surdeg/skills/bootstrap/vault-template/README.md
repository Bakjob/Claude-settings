# [PROJECT_NAME] vault

This folder is [PROJECT_NAME]'s memory: the hard rules, how to run it,
settled decisions, today's status and a dated history of the work. Read
`hard-rules.md` and `status/current-status.md` before starting real work.

## Layout

- **`hard-rules.md`**: the enforced rules. Short, and every rule exists
  because breaking it once cost something.
- **`running.md`**: how to install, run, build, test and smoke-test.
- **`decisions/`**: numbered records of calls that rule out an alternative.
  Never renumbered or deleted, only superseded. `decisions/README.md` holds
  the template and the index.
- **`progress/`**: one dated file per real chunk of work
  (`YYYY-MM-DD-slug.md`), append-only.
- **`status/current-status.md`**: what is true right now. Edited in place.
- **`status/open-questions.md`**: what is still undecided.

## Maintenance

After a real chunk of work, add a `progress/` entry, update
`status/current-status.md` if what's true changed, and add a decision if
something was settled. Status says what is true now; progress says how it got
there. If they disagree, fix status.
