# Kvarngatans Bageri

The website of a small sourdough bakery: opening hours, the week's bread,
ordering for pickup and the bakery's story. Astro, static, hosted on Netlify.

This file is process and collaboration context: how to work in this repo,
not what the project does. Everything about the project itself (the hard
rules, how to run it, settled decisions, current status) lives in
`docs/` (see below); todos live in `docs/status/todo.md`. If you're
about to write code and haven't read docs/hard-rules.md this
session, read it first: it's short, and every rule in it exists because
breaking it once already cost a pass to fix.

**This file stays short, on purpose.** If it grows past a few hundred lines
(design decisions, status updates, bugs, a full pass-by-pass history all
bolted on over time), split the history out into a dated file and leave a
pointer. If you're about to add a design decision, a status update, a bug,
or more than a line or two of "how the project works" to this file, it
almost certainly belongs in `docs/` instead, with a pointer left here at
most.

---

## Language rule

- **Code, identifiers and comments:** English.
- **Docs:** English.
- **Everything visitors see:** Swedish.
- **Commit messages:** English, Conventional Commits (`feat: add pickup form`).
- **Talking to the user:** Swedish.

**Never use em dashes.** Use a colon, a comma, parentheses, or a full stop
instead. This applies to code comments, docs, and UI strings alike.

---

## Project docs: read this before anything design-related

`docs/` is this project's memory: the hard rules, how to run it, the
core idea, settled decisions, the current status, and a dated progress log.
Start with docs/README.md for the layout. Load-bearing entry points
(adapt to what actually exists):

- `docs/hard-rules.md`: the enforced engineering rules. Read
  before touching core code.
- `docs/running.md`: how to actually run and smoke-test the
  project.
- `docs/quality-targets.md`: the SEO, accessibility and performance bar.
- `docs/status/current-status.md` and `.../open-questions.md`: read
  before starting real work; they describe today.
- `docs/decisions/`: numbered ADRs (architecture decision records)
  for anything genuinely settled, so nobody re-opens a fought-out question
  by accident.
- `docs/setup.md`: the answers this setup was generated from.
- Todos: `docs/status/todo.md` (see below).

**Keep it current.** After a real chunk of work, update the docs: a new
numbered decision file if something was actually settled, an edit in place
to the current-status doc if what's true about the project changed. (This
is a light vault: no dated progress log.)
Use a dedicated skill/agent for this if one exists, rather than letting
updates slide.

### If a request contradicts something settled in the decisions log

Do not silently comply and do not silently refuse. Say plainly what the
decisions entry says and that this request goes against it, then ask:
**"Are you sure?"** This is not friction for its own sake: the decisions
log exists specifically so nobody, human or Claude, quietly re-opens a
question that was already fought out, and asking once, explicitly, is what
keeps both sides certain a direction change is a direction change and not
a drift.

---

## Todos

There is no issue tracker. Open work lives in `docs/status/todo.md` as a
checklist, newest at the top. Add a line when work is discovered, tick it
when it is verified (not just deployed), and delete ticked lines once
`current-status.md` reflects them.

---

## Push back, don't be a yes-man

If a design choice is requested that looks like it will require tearing up
working systems for a small gain, or that conflicts with this file's hard
rules or the reasoning in the decisions log, **say so before building it.**
Explain concretely what it costs: which systems have to be reworked, what
hard rule or settled decision it conflicts with, and why the tradeoff looks
bad. Then ask for an explicit confirmation before proceeding.

---

## Clean code standard

- **Names say what a thing is.** A variable, function, or class name should
  make a comment unnecessary. If you need a comment to explain what code
  does rather than why, rename instead of commenting.
- **Small functions, single responsibility.** A function does one thing. If
  describing it needs "and", it is probably two functions.
- **No dead code.** Delete unused functions, commented-out blocks, and
  unused variables rather than leaving them "in case." Git history is the
  safety net, not a comment block.
- **DRY, but not at the cost of the wrong abstraction.** Three similar
  lines used in three places do not automatically need a shared helper; a
  premature abstraction that has to be unwound later costs more than the
  duplication it avoided. Extract when a THIRD real use case shows up, not
  before.
- **Comments explain WHY, never WHAT.** A comment justifying a non-obvious
  constraint, a workaround, or a subtle invariant is worth writing. A
  comment restating what the next line does is not.
- **Consistent formatting**, matching whatever the surrounding file already
  does. Do not introduce a second style into a file that already has one.
- **No half-finished implementations.** A feature either works end to end
  or it is not committed. No feature flags, no silent fallbacks for a case
  that cannot currently happen, no TODO standing in for logic that should
  exist.
- Where this ever conflicts with a project-specific hard rules doc, the
  project-specific rules win, because they encode lessons that project
  already paid for.

---

## Git workflow

One person and Claude work in this repository, straight on `main`:

- **Commit often, at reasonable points.** A commit should be one coherent,
  working change. If you can't describe the commit in one sentence, it is
  probably more than one commit.
- **Work happens on `main`.** No branches or pull requests for routine work;
  a branch only for an experiment that might be thrown away.
- **Claude commits on its own once a change is verified** (build,
  typecheck and lint pass), **and asks before every push**: a push to
  `main` deploys the site through Netlify.
- Commit messages use Conventional Commits (`feat:`, `fix:`, `docs:`,
  `chore:`), English, no em dashes.

---

## Plugins, agents and skills for this project

Enabled in `.claude/settings.json`; invoke them directly instead of
re-deriving their logic:

- **`bakjob-core`**: `git-checkpoint`, `vault-update`, `new-decision`,
  `architect` for hard plans, `test-runner` for build, typecheck and lint.
- **`bakjob-web`**: `design-taste-frontend` and `redesign-skill` for the
  look, `visual-check` after every UI change, `seo-a11y-auditor` against
  `docs/quality-targets.md`, `web-deploy` for Netlify.

They read this project's values from this file and from
`docs/hard-rules.md`, `docs/running.md`, `docs/quality-targets.md` and the
rest of `docs/`, so keep those current.

