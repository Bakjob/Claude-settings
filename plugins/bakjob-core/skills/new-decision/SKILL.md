---
name: new-decision
description: Scaffold a new numbered decision record in the project's vault decisions/ folder when something about its design or architecture gets genuinely settled. Use whenever a real "we're doing X, not Y, because Z" call gets made, not for routine implementation choices.
---

# New decision

Creates one new file in `DOCS/decisions/`, correctly numbered and linked. `DOCS` is the docs folder the project's `CLAUDE.md` names, usually `vault/`; below it's written as `vault/`.

## When this applies

A decision record is for calls that **foreclose an alternative** and that a future session (human or Claude) might otherwise re-litigate or accidentally reverse: a direction change, a system getting cut or replaced, a hard constraint being adopted. It is not for routine implementation details that don't need defending later (variable names, which function a helper lives in, etc.) — those don't need a decision record, just good code.

If in doubt: would someone reading this file in three months, about to propose the opposite, need to be talked out of it? If yes, it's a decision.

## Steps

1. Read `vault/decisions/README.md` for the current template and the index table's highest existing number.
2. Pick the next number (zero-padded to 4 digits, e.g. `0009`) and a short kebab-case slug for the filename: `NNNN-short-slug.md`.
3. Write the file using the template in `vault/decisions/README.md`: **Decision** (one or two sentences, what is now true), **Why** (the actual reasons — name the incident or constraint if there was one), **Rules out** (what this forecloses), **See also** (links to a fuller writeup elsewhere in the vault, and to any decision this supersedes or is superseded by).
4. If this decision **supersedes an earlier one**, edit that earlier file's `Status` line to point at the new one; do not delete the old file or its reasoning.
5. Add a row to the index table in `vault/decisions/README.md`, in number order.
6. If a fuller explanation belongs somewhere (a whole new system, or a substantial change to an existing one), also update or create the relevant design doc elsewhere in the vault, and link it from the decision file. The decision file itself should stay a page; put the depth there, not here.
