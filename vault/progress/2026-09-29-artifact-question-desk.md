# 2026-09-29: artifact-question-desk skill

## What changed

- New skill `skills/artifact-question-desk/`: `SKILL.md` describes the format
  and how Claude reads the answers back, and `question-desk.html` is a
  working page template.
- `README.md` skills table and `status/current-status.md` list it.
- `CLAUDE-template.md`'s push-back section points at it for grilling rounds
  and for plans that still have open decisions.

## Why

In the Space Hex project, Claude built a "Vision Desk" artifact to collect
answers to open design questions. The user said it was the best way yet to
be asked questions, and asked for every future artifact to use it: "I like seeing at the top how many
questions and ideas I have left to answer, and a copy as text button. Then
multiple choice questions, and a new ideas form at the bottom." (translated
from Swedish)

## How it follows the library's rules

Per [decision 0001](../decisions/0001-bracket-placeholder-convention.md), the
Space Hex page was not copied in as a filled-in example. The template keeps
the page's working code (rendering, auto-save to the artifact's `db`,
counters, Copy as text, pitch form) and turns all content into
`[BRACKETED_PLACEHOLDERS]` in four constants at the top of the script. Its
JavaScript passes `node --check`. It hasn't been published as an artifact
from this repo, since it's the unfilled template.
