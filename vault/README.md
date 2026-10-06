# Vault

This is the second brain for **Claude-settings itself** — the meta-project of
maintaining this template library, not a vault for any project you copy
templates into. Open this `vault/` folder as its own Obsidian vault.

Why a separate vault from the root `README.md`: the root README explains what
exists and how to use it (a reference, stays stable). This vault records how
the library got that way and what's still open — decisions, a dated history,
today's status, and a backlog. The root README rarely changes shape; this
vault changes after almost every real session.

**This is also the reference implementation.** `templates/agents/vault-scribe.md`,
`templates/skills/vault-update/` and `templates/skills/new-decision/` (and
bootstrap's `templates/vault/` skeleton) describe a
`decisions/` / `progress/` / `status/` vault layout for you to copy into
*other* projects. This vault uses that exact layout on itself. If you change
the layout here, update those templates to match, and vice versa.

## Layout

- **`decisions/`** — Numbered, permanent records of calls that foreclose an
  alternative (e.g. "skills use bracket placeholders, not a second
  'example' copy of each file"). One file per decision, never renumbered,
  never deleted, only superseded. `decisions/README.md` holds the template
  and the index table.
- **`progress/`** — A dated, append-only log. One file per work session,
  named `YYYY-MM-DD-slug.md`: what changed and why. Never edit a past
  entry's conclusions — if a later session finds one wrong, say so in the
  new entry instead.
- **`status/current-status.md`** — What's actually in the library right now,
  at a glance. Edited in place; describes today, not history.
- **`status/open-questions.md`** — Things not yet decided about the library
  itself (not project ideas — those go in `ideas/`).
- **`ideas/`** — Backlog of candidate agents/skills/templates not yet built,
  and things worth genericizing further. Not permanent like `decisions/`;
  prune entries once they're built or dropped.

## Maintenance rules

- After a real chunk of work on this repo (added or reworked an agent/skill,
  restructured something, made a real call about how the library should
  work), add a `progress/` entry and update `status/current-status.md` if
  what's true about the library changed. Use `vault-scribe` /
  `vault-update` (copied into a consuming project) as the model for how to
  do this; for this repo, just do it directly or ask Claude to.
- A decision record is for calls a future session might otherwise
  re-litigate: "why did we generalize X instead of deleting it," "why does
  Linear stay concrete instead of templated," etc. Routine edits (fixing a
  typo, rewording a description) don't need one.
- Don't let `status/current-status.md` and `progress/` drift apart — status
  is the current snapshot, progress is how it got there. If they disagree,
  status wins for "what's true now," progress wins for "what happened and
  why."
