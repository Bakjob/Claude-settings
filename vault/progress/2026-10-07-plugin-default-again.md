# 2026-10-07: Plugin install is the default again (#32)

## What changed

- Round 12's **Install mode** question recommends Plugins; Copy is the
  option (and still the only mode in pasted mode).
- `generate.md` step 4 lists plugin mode first as the default.
- README leads with the plugin install; copying is the collapsed alternative.
- Examples restored to their plugin-mode form (`enabledPlugins` in
  `settings.json`).
- Decision 0010 supersedes 0009. Copy mode, `feed`/`doctor` handling of both
  modes and the SKILL.md frontmatter fix from #29 are kept.
- `bakjob-surdeg` 0.5.0 -> 0.6.0.

## Not done

Bootstrap has not been run on a real project in either mode (same open item
as #3, #4 and #29).
