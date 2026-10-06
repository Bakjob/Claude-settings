# 2026-10-07: Bootstrap copies into the project by default (#29)

## What changed

- Round 12 got an **Install mode** question: Copy (default, and the only
  mode in pasted mode) or Plugins.
- `generate.md` step 4: copy mode copies each accepted plugin's skills and
  agents, plus `bootstrap`, `feed` and `doctor`, into `.claude/`, writes no
  plugin keys, and records plugin versions in the answer sheet. Plugin mode
  is the old behaviour.
- `SKILL.md`: `LIB` resolution (pasted folder, else the marketplace clone,
  else a shallow clone) and a verify step that skips the copied `bootstrap`
  skill's own placeholders.
- `feed` adds, removes and refreshes copies; `doctor` reports by install mode.
- `CLAUDE-template.md` section renamed "Skills and agents for this project".
- README leads with pasted mode; examples lose their plugin keys and say the
  `.claude/` copies are left out.
- Decision 0009 supersedes the install mechanism of 0008.
- `bakjob-surdeg` 0.4.0 -> 0.5.0.

## Verified

`claude plugin validate` passes for the marketplace and for `bakjob-surdeg`,
`-github`, `-linear`, `-web`, `-game`. A scratch project filled by hand
following the new step 4 has every skill and agent under `.claude/`, the
`feed`/`doctor` links to `../bootstrap/` resolve, and the only placeholders
left are the copied `bootstrap` skill's own templates (hence the verify
exception).

## Found on the way

- `bootstrap/SKILL.md`'s `description` had a `: ` that broke the YAML, so the
  skill loaded without its description. Fixed here. Its always-on cost is now
  ~510 tokens, not the ~120 measured before, because the description is
  finally counted.
- `bakjob-core`'s `architect` and `vault-scribe` agents fail the same
  validation: #30.

## Not done

Bootstrap in copy mode has not been run on a real project yet (same open item
as #3 and #4).
