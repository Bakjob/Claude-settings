# [PROJECT_NAME]

[One or two lines: what this project is, in plain language. The elevator pitch.]

This file is process and collaboration context: how to work in this repo,
not what the project does. Everything about the project itself (the hard
rules, how to run it, settled decisions, current status) lives in
[DOCS_DIR] (see below); bugs and todos live in [ISSUE_TRACKER]. If you're
about to write code and haven't read [DOCS_DIR]/[HARD_RULES_FILE] this
session, read it first: it's short, and every rule in it exists because
breaking it once already cost a pass to fix.

**This file stays short, on purpose.** If it grows past a few hundred lines
(design decisions, status updates, bugs, a full pass-by-pass history all
bolted on over time), split the history out into a dated file and leave a
pointer. If you're about to add a design decision, a status update, a bug,
or more than a line or two of "how the project works" to this file, it
almost certainly belongs in [DOCS_DIR] instead, with a pointer left here at
most.

---

## Language rule

**Everything in this project is written in [LANGUAGE].** Code, comments,
doc comments, documentation, commit messages, and every string the user
ever sees.

**Never use em dashes.** Use a colon, a comma, parentheses, or a full stop
instead. This applies to code comments, docs, and UI strings alike.

*(Adjust or drop this section if the project's language/style conventions differ.)*

---

## Project docs: read this before anything design-related

[DOCS_DIR] is this project's memory: the hard rules, how to run it, the
core idea, settled decisions, the current status, and a dated progress log.
Start with [DOCS_DIR]/README.md for the layout. Load-bearing entry points
(adapt to what actually exists):

- `[DOCS_DIR]/[HARD_RULES_FILE]`: the enforced engineering rules. Read
  before touching core code.
- `[DOCS_DIR]/[RUNNING_FILE]`: how to actually run and smoke-test the
  project.
- `[DOCS_DIR]/status/current-status.md` and `.../open-questions.md`: read
  before starting real work; they describe today.
- `[DOCS_DIR]/decisions/`: numbered ADRs (architecture decision records)
  for anything genuinely settled, so nobody re-opens a fought-out question
  by accident.
- Bugs and todos: tracked in [ISSUE_TRACKER], not in the docs directory.

**Keep it current.** After a real chunk of work (a pass, a bugfix worth
remembering, a decision), update the docs: a dated entry in a progress log,
a new numbered decision file if something was actually settled, an edit in
place to the current-status doc if what's true about the project changed.
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

## Issue tracker

**Every new feature, bug, and improvement goes through [ISSUE_TRACKER]**
(workspace/project: [TRACKER_WORKSPACE], team: [TRACKER_TEAM]), not just
bugs, not in the docs, not in a file in this repo. Labels distinguish the
kinds of work; the workflow below is identical for all of them.

- **Starting a real chunk of new work creates the issue**, if one doesn't
  already exist, at the moment work begins, not after.
- **Move an issue to In Progress AND set its assignee in the same action,
  before the first edit, not batched in alongside the commit or PR.** A
  state flip with no assignee is just as misleading as no state flip at
  all. This isn't ceremony: collaborators read the tracker's live state to
  decide what's safe to pick up, and an issue still sitting in Todo (or
  claimed by nobody) while it's actually mid-edit is a false signal that
  invites two people starting the same fix at once.
- **Everyone who touches an issue puts their name on it.** Anyone may fix
  any issue: who is allowed to work on what is not the question. The
  question is who DID, and state alone ("In Progress", "Done") does not
  answer it. The assignee field does. Never overwrite a name that is
  already there, that person is on it; an issue left unassigned is the
  actual failure, not one assigned to someone else.
- **If a commit fixes or works toward a tracker issue, name it in the
  message** (e.g. `PROJ-30`), so it's clear from git log alone which
  commits are trying to fix what.
- **Sync the tracker the moment ANYTHING merges, whoever merged it.** When
  a `git pull`, `gh pr list`, or similar surfaces a PR that merged without
  you, check the matching issue then, not only when asked.

### The test loop lives in the tracker, not in the PR

Collaborators merge, not Claude (see "Git workflow" below). What changes is
that they don't have to manually test something before merging; whatever
manual verification is still needed moves to the tracker issue instead of
gating the merge:

- **PR descriptions get no "Test plan" checklist.** If a sentence about to
  go in a PR body starts "To verify" or ends in a row of `- [ ]` lines,
  stop, that belongs on the tracker issue instead (its description or a
  comment), as real markdown checkboxes, not prose. A PR body may SAY that
  manual verification is needed and point at the issue; it may not contain
  the checklist itself.
- **Every PR gets [PR_TITLE_CONVENTION]**, e.g. a leading gitmoji matching
  its label (🐛 bug, ✨ feature, ♻️ improvement, 📝 docs), repeated next to
  "Ready to merge" at the end of the body once it actually is. Adapt or
  drop this if the project has no such convention; the point is that
  anyone scanning an open-PR list can tell what a PR is and whether it's
  waiting on the click without opening each one.
- **"Done" means verified, not merged.** A merged fix whose issue still
  needs a human to check something moves to **In Review** (not Done, not
  back to In Progress), with a checklist attached, until a human confirms
  it or checks the boxes themselves.
- **Automatic checks are Claude's to run, manual checks are the human's to
  confirm.** Anything an automated test/smoke-test can actually prove is
  Claude's job to run and read before the issue ever reaches In Review, not
  something to leave on its checklist waiting for a human. Only put a
  checkbox on the issue for what genuinely needs eyes and hands (how
  something feels, a visual glitch, a multiplayer/multi-user scenario); if
  the whole issue is provable by automated test, run it and go straight to
  Done. Never move a checklist item or the issue itself to Done on
  judgement call alone when it needed a human, only the human saying so
  does that.
- If the tracker has GitHub integration that auto-transitions issues off
  PR state, **put the issue ID in the PR TITLE itself**, not only the body
  or individual commit messages, that's what the integration usually
  matches on. Still worth a manual check after any merge noticed after the
  fact, the automation is one more thing that can silently fail to match.

**The full In Review loop, and the exact phrases that drive it:**

- **A plain statement that something is fully tested / confirmed working**
  moves that issue straight from In Review to **Done**. Act on it in the
  same turn, don't ask for confirmation first.
- **A report that something doesn't work, or is incomplete, moves that
  issue from In Review back to In Progress.** It stays there while the
  follow-up fix is built; once merged, it returns to In Review
  automatically (same as any other merge), waiting for testing again. This
  can repeat more than once for the same issue; that's normal.
- **Every issue sitting In Review should have a checklist of what still
  needs a human to check**, as real markdown checkboxes, not prose.

---

## Push back, don't be a yes-man

If a design choice is requested that looks like it will require tearing up
working systems for a small gain, or that conflicts with this file's hard
rules or the reasoning in the decisions log, **say so before building it.**
Explain concretely what it costs: which systems have to be reworked, what
hard rule or settled decision it conflicts with, and why the tradeoff looks
bad. Then ask for an explicit confirmation before proceeding. Agreeing
immediately and building the wrong thing well is not helpful; check the
project's own progress/decisions log for past cases where the right move
was diagnosing the real problem before writing code, not building the
first request literally. Grill the choice, get a real answer, then build
exactly what was confirmed.

When the grilling is more than a few questions, or a plan published as an
artifact still has open decisions, build that artifact with the
`artifact-question-desk` skill, so the answers come back in one place.

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

## Configurable options

Anything that's genuinely a user-configurable option belongs in one place
(a settings menu, a config file), not bolted on as a one-off control
somewhere else. [Adapt: point at the project's actual settings system and
any UI convention it follows.]

---

## Git workflow

More than one person (human and/or Claude) works in this repository. The
rules:

- **Commit often, at reasonable points.** A commit should be one coherent,
  working change: a bugfix, one pass's worth of a feature, a docs update.
  Not every keystroke, and not a whole day's unrelated work bundled into
  one commit either. If you can't describe the commit in one sentence, it
  is probably more than one commit.
- **Work happens on a branch, not on `main`.** Branch off `main` for
  anything beyond a trivial one-line fix. Name branches by what they do:
  `feature/<slug>`, `fix/<slug>`, `chore/<slug>`.
- **Merge into `main` through a pull request**, opened when a chunk of work
  is actually done, or whenever it's simply a good moment to land what
  exists so far (a branch that lives for weeks accumulates conflict risk
  for no benefit). Small, frequent PRs beat one enormous one.
- **Commit, branch, push, and open the PR automatically, without asking
  first, once a change is actually verified** (tests pass, or plain
  code-review-level confidence for a docs-only change).
- **Merging is the one step this does NOT cover.** Open the PR, run
  whatever verification exists, say plainly that it's ready; the actual
  merge click on GitHub (or equivalent) is the human's, every time, no
  exceptions carved out by "it was only a docs change" or "they said merge
  fast earlier."
- Commit messages follow the repo's existing tone: direct, states what
  changed and often why, no filler, no em dashes.

---

## Agents and skills for this project

[List any project-specific custom agents/skills here, e.g.:
`.claude/agents/`: ... `.claude/skills/`: ...
Invoke these directly instead of re-deriving their logic each time.]

---

## Filling in this template for a new project

Replace every bracketed placeholder above, then delete this section:

- `[PROJECT_NAME]`, elevator pitch
- `[LANGUAGE]` (drop the section entirely if not applicable)
- `[DOCS_DIR]` (e.g. `vault/`, `docs/`, `notes/`), `[HARD_RULES_FILE]`,
  `[RUNNING_FILE]`
- `[ISSUE_TRACKER]`, `[TRACKER_WORKSPACE]`, `[TRACKER_TEAM]`
- `[PR_TITLE_CONVENTION]` (or delete that bullet if the project has none)
- The "Configurable options" and "Agents and skills" sections, once the
  project actually has those things.

Sections that don't fit a given project (e.g. no settings system yet, no
custom agents yet) can be deleted rather than left as placeholders; add
them back when they become real.
