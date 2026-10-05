# Claude-settings

A personal library of reusable [Claude Code](https://claude.com/claude-code)
configuration: a `CLAUDE.md` template, custom subagents, and custom skills.
Everything here is written as a **fill-in-the-blanks template**, meant to be
copied — in whole or in part — into a new project repo and then adapted to
that project, not used as-is from this repo.

## What's in here

```
CLAUDE-template.md   — root-level project instructions template
agents/               — custom subagents (.claude/agents/*.md in a project)
skills/               — custom skills (.claude/skills/*/SKILL.md in a project)
vault/                — this repo's OWN second brain (Obsidian) — see below
```

### `CLAUDE-template.md`

The starting point for a new project's `CLAUDE.md`: process and collaboration
rules (docs layout, issue-tracker workflow, git workflow, code standards),
not what the project does. Copy it to the new repo's root as `CLAUDE.md` and
fill in every `[BRACKETED_PLACEHOLDER]` — the template itself ends with a
checklist of exactly which placeholders to fill and which sections to delete
if they don't apply yet.

### `agents/`

Each file is a Claude Code subagent definition. Copy the ones relevant to a
project into `.claude/agents/` there, then fill in its placeholders.

| Agent | What it's for |
|---|---|
| `architect.md` | Read-only planning agent for the hardest, multi-subsystem problems — produces a written plan, never code. |
| `config-value-auditor.md` | Finds hardcoded tuning/config values that should live in one central config file instead. |
| `seo-a11y-auditor.md` | Audits a site against its own stated SEO / accessibility (WCAG) / performance (Lighthouse) targets. |
| `smoke-test-runner.md` | Runs a project's smoke test and reports pass/fail per check, with the real numbers. |
| `test-runner.md` | Writes and runs automated tests (unit/E2E/build/lint) after a change, reports what passed and what has no coverage. |
| `vault-scribe.md` | Updates a project's `vault/` after a real chunk of work — the agent form of `skills/vault-update`. |

### `skills/`

Each folder is a Claude Code skill (`SKILL.md` plus any supporting files).
Copy the ones relevant to a project into `.claude/skills/`.

| Skill | What it's for |
|---|---|
| `git-checkpoint` | Checks whether now is a reasonable point to commit / branch / open a PR. |
| `linear-dev-workflow` | Working [Linear](https://linear.app) issues: pulling context, blockers, status/assignee flow, the In Review → Done loop. **Kept concrete** (real `mcp__linear__*` tool calls) rather than templated to a generic tracker — see [decision 0003](vault/decisions/0003-linear-stays-concrete.md). Swap in the project's team/prefix/identifiers. |
| `static-site-deploy` | Deploy checklist for a static-export site to shared/FTP-style hosting (DNS, SSL, redirects, post-launch SEO steps). |
| `new-decision` | Scaffolds a new numbered entry in a project's `vault/decisions/`. |
| `smoke-test` | Runs a project's smoke test and reads the output against each check's own pass criterion. |
| `test-all-branches` | Rebuilds a disposable local branch combining every currently open PR, for testing everything in flight at once. |
| `vault-update` | Routine vault maintenance after a work session — decide decision vs. progress entry vs. status edit. |
| `content-page-family` | Pattern for a family of pages that share structure but differ in content (case studies, product pages, per-item doc pages). |
| `design-taste-frontend` | Anti-slop frontend design skill for landing pages/portfolios/redesigns — infers a design direction from the brief and pushes back on default LLM aesthetics. Already fully generic, use as-is. |
| `redesign-skill` | Audit-first checklist for upgrading an existing site's design (typography, color, layout, states, content) without breaking functionality. Already fully generic, use as-is. |
| `artifact-question-desk` | The default shape for any claude.ai Artifact that asks the user something: progress counters and Copy as text on top, multiple-choice questions with context, ideas to sort, and a new-ideas form at the bottom, auto-saved to the artifact's db so Claude reads the answers back. Ships a working `question-desk.html` template. |

## How to use this in a new project

1. Copy `CLAUDE-template.md` to the new repo as `CLAUDE.md`, fill in its
   placeholders, delete sections that don't apply yet.
2. Copy whichever `agents/*.md` files are relevant into `.claude/agents/`,
   and whichever `skills/*/` folders are relevant into `.claude/skills/`.
3. Fill in every `[BRACKETED_PLACEHOLDER]` in the copied files — each one
   names what to put there (project name, hard-rules file path, docs
   directory, tracker identifiers, run/build commands, etc.).
4. If the new project wants the same `vault/` pattern these templates
   assume (`decisions/`, `progress/`, `status/`), build one there — see
   "The vault pattern" below for the shape. That new vault is the new
   project's own second brain, separate from this repo's.

Not every agent/skill applies to every project — copy only what fits, and
delete the rest of the placeholder text once filled in (per
`CLAUDE-template.md`'s own "filling in this template" section).

## The vault pattern (for projects you copy this into)

Several templates here (`vault-scribe`, `vault-update`, `new-decision`,
`architect`) assume a project keeps a `vault/` folder as its living memory:
hard-earned decisions, a dated history of work, and today's status, so
nothing has to be re-derived or re-litigated every session. The shape:

- **`vault/decisions/`** — one numbered file per settled call that forecloses
  an alternative, plus an index table in `vault/decisions/README.md`. Never
  renumbered or deleted, only superseded.
- **`vault/progress/`** — one dated file per work session
  (`YYYY-MM-DD-slug.md`), append-only.
- **`vault/status/current-status.md`** and **`open-questions.md`** — what's
  true right now and what's still unresolved, edited in place.
- Whatever design/system-doc subfolders the project actually needs beyond
  that (this repo's own vault only needed `ideas/` on top of the three
  above — a game project might want `game-design/`, a web project might
  want none).

This repo's own `vault/` (below) is the reference implementation of that
shape, applied to maintaining this library rather than to a game or a
website.

## This repo's own vault

`vault/` at the root of *this* repo is not an example for you to copy — it's
this library's own second brain, tracking the library's history, open
questions, and backlog. Open it as its own Obsidian vault
(Obsidian → Open folder as vault → `vault/`). Read `vault/README.md` first;
it explains the layout and the maintenance rules in full.
