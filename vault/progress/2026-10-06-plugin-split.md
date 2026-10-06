# 2026-10-06: Per-project plugins, no placeholders in shared skills

## What changed

- The marketplace now holds five plugins under `plugins/`: `bakjob-surdeg`
  (bootstrap, per user) and `bakjob-core`, `bakjob-github`, `bakjob-linear`,
  `bakjob-web` (per project). `templates/` and the root `skills/` are gone.
- Every shared skill and agent reads project values from the project's own
  files instead of `[PLACEHOLDERS]`. The contract is
  `plugins/bakjob-surdeg/skills/bootstrap/project-facts.md`: `CLAUDE.md`
  (name, docs folder, tracker identifiers, git workflow),
  `DOCS/hard-rules.md` (incl. the config file line), `DOCS/running.md`
  (commands, smoke test checks, deploy), `DOCS/quality-targets.md` and
  `DOCS/design/page-families.md` for web. A missing value is asked for once
  and written down.
- Bootstrap: round 12 proposes plugins instead of single templates; new
  questions for page families, a config file and the web quality bar;
  `generate.md` writes the fact files and runs
  `claude plugin install <p>@bakjob --scope project`, then adds
  `extraKnownMarketplaces` itself (the install only writes
  `enabledPlugins`, found while testing). Pasted mode copies plugin contents
  into `.claude/` instead.
- This repo enables `bakjob-github` for itself instead of a copied skill.
- Decision 0008 supersedes 0001 and 0005.

## Verified

All five plugins and the marketplace pass `claude plugin validate`. In an
isolated config dir: the marketplace added from the local checkout,
`bakjob-surdeg` installed at user scope, `bakjob-core` and `bakjob-web` at
project scope in a scratch git repo (written to its
`.claude/settings.json`). Always-on cost: surdeg ~120 tokens, core ~1,100,
web ~350.

## Not done

A full bootstrap run on a real project (#3, #4).
