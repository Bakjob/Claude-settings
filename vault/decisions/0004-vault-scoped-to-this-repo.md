# 0004: This repo's `vault/` documents the meta-project, not a stand-in for the vault pattern the templates describe

**Status:** Active

**Decision:** The `vault/` folder at the root of this repo is this repo's
own second brain (decisions and history about the *template library*). It is
not an example vault meant to be copied wholesale into a consuming project —
`agents/vault-scribe.md`, `skills/vault-update.md`, and `skills/new-decision.md`
are the portable templates for that; a new project builds its own `vault/`
from those, shaped around its own subject matter.

**Why:** Conflating the two would mean either polluting this repo's own
history with fake example content, or leaving the actual templates
under-specified because "just look at the real vault/ for an example."
Keeping them separate lets this vault be genuinely useful for maintaining
the library while the templates stay clean fill-in-the-blanks files.

**Rules out:** Deleting or genericizing this `vault/` folder's own content
when copying the repo's *agents/skills* elsewhere — this vault stays behind;
only `agents/`, `skills/`, and `CLAUDE-template.md` get copied out.

**See also:** [0001](./0001-bracket-placeholder-convention.md)
