---
name: vault-update
description: Update [PROJECT_NAME]'s vault (vault/) after finishing a chunk of work — write a dated progress entry, refresh vault/status/current-status.md if what's true about the project changed, and add a decisions/ entry if something was actually settled. Use at the end of a work session, after a pass, after a bugfix worth remembering, or whenever asked to "update the vault" / "log this."
---

# Vault update

Keeps `vault/` current. Read `vault/README.md` first if it's been a while; the layout and maintenance rules live there and this skill defers to them.

## Steps

1. **Look at what actually changed.** Run `git status` and `git diff` (staged and unstaged) plus `git log` for anything already committed this session. Don't rely solely on your own memory of the conversation; the diff is ground truth.

2. **Decide what kind of entry this is**, possibly more than one:
   - Something was **settled** that forecloses an alternative or reverses an earlier call → new file in `vault/decisions/`, numbered one higher than the last entry in `vault/decisions/README.md`'s index (never reuse or renumber), following the template in that same README. Add it to the index table too.
   - Real work happened this session, decision or not → new file in `vault/progress/`, named `YYYY-MM-DD-slug.md`. Keep it factual: what changed, why, what it fixed or broke. Append-only, never rewrite a past entry's conclusions, only add a note in the new entry if something earlier turned out wrong.
   - What's **currently true** about the project changed (a feature now works, a bug is now fixed, an open question got answered) → edit `vault/status/current-status.md` and/or `vault/status/open-questions.md` **in place**. These describe today; do not leave a stale fact sitting next to the new one.
   - How a system works or why changed → edit the owning design doc elsewhere in the vault rather than creating a duplicate.

3. **Write in the vault's existing voice**: direct, reasons before conclusions, no padding. A progress entry is a paragraph or two. Cross-link related files by relative path the way the rest of the vault already does.

4. **Do not touch** [HARD_RULES_FILE]'s hard-rule sections, or any file the vault README marks as human-owned, as part of this skill; those follow different edit rules.

5. **If nothing vault-worthy happened** (a typo fix, pure formatting), say so instead of manufacturing an entry. A padded vault is as bad as a stale one.

For a larger or more ambiguous update, delegate to the `vault-scribe` agent instead of doing all of the above inline — it's built for exactly this and will do the git inspection and file classification itself.

---
*Template — fill in `[PROJECT_NAME]` and `[HARD_RULES_FILE]` for the target project. The vault layout this skill assumes is the same one described in this repo's own `vault/README.md` — see the root README for the full pattern.*
