# Generating the project setup

Runs only after the user confirmed the summary. `SKILL_DIR`, `LIB`, `TARGET`
and the answer sheet come from [SKILL.md](SKILL.md) and
[questions.md](questions.md). The files written here are what the plugins
read, so they must match [project-facts.md](project-facts.md).
`DOCS` below means the docs folder the user chose (`vault/` or `docs/`).

Write everything in the languages the user picked: docs language for
`CLAUDE.md` and the vault, commit language for the first commit. Template
files are in English; translate them when the docs language is not English,
but keep the file names and the section headings that
[project-facts.md](project-facts.md) names as they are, so the plugins find
them.

## 1. CLAUDE.md

Start from `SKILL_DIR/CLAUDE-template.md` and write `TARGET/CLAUDE.md`:

- Fill every placeholder from the answer sheet. `[HARD_RULES_FILE]` is
  `hard-rules.md`, `[RUNNING_FILE]` is `running.md` (both created in step 3).
- **Language rule:** state the four languages separately (code, docs, UI,
  commits) plus the chat language. Drop the em dash line if the user dropped
  that rule.
- **Issue tracker:** pick the variant in section 5 below.
- **Push back:** "Grill me" keeps the section as is. "Flag clear problems"
  shortens it to the first paragraph. "Build what I say" replaces
  it with one line: flag a conflict with a decision or hard rule once, then
  build what was asked. Drop the `artifact-question-desk` sentence if the
  user chose terminal questions.
- **Clean code:** keep it for "The template's clean code standard"; for "Lighter", keep only
  the names, no-dead-code and comments-explain-why bullets.
- **Git workflow:** rewrite the bullets to match the answers, not the
  template's defaults: branch strategy, who merges, what Claude may do
  without asking, commit style, PR title convention. If there is no git,
  replace the section with one line saying so.
- **Configurable options:** keep only if the project will have a settings
  system; otherwise delete.
- **Plugins:** list the plugins step 4 enables and, one line each, what
  they bring.
- Delete the template's "Filling in this template" section.

## 2. Vault

Skip if Vault = None (then `DOCS` is just a folder for `hard-rules.md` and
`running.md`, default `docs/`).

Copy `SKILL_DIR/vault-template/` to `TARGET/DOCS`, fill its placeholders, then:

- **Light:** delete `progress/` mentions from `README.md` and don't create the
  folder.
- **Full:** create `progress/YYYY-MM-DD-bootstrap.md` (today's date) with the
  whole answer sheet as a table, so a later session can see why the setup
  looks the way it does.
- Each extra folder gets a `README.md` saying what belongs there. For
  `game-design/`, seed sections: core loop, mechanics, progression, tuning
  values (pointing at the config file once it exists).
- `status/current-status.md`: "Bootstrapped on YYYY-MM-DD, no code yet" (or
  what pre-flight found in existing code).
- `status/open-questions.md`: every fact the enabled plugins need that
  can't be known yet, as "add X to running.md once Y exists".

## 3. The fact files

Write these exactly in the shape [project-facts.md](project-facts.md)
describes.

- `DOCS/hard-rules.md`: short. Only rules that follow directly from the
  answers (languages, em dashes, commit style, "never commit to main" if
  feature branches were chosen) plus stack-specific ones that are always
  true, for example for Godot "never hand-edit `.import` files or `.godot/`",
  for Unity "never edit `.meta` files by hand; keep them committed". If
  Config file = Yes, add the **Config file** line with the path and the
  categories of value that belong there.
- `DOCS/running.md`: the `## Setup`, `## Dev`, `## Build`, `## Test`,
  `## Lint and format`, `## Smoke test` and `## Deploy` sections, with the
  commands for the stack and package manager. For a project with no code
  yet, list the commands the chosen stack will use and say they apply once
  it is scaffolded. A section with nothing known yet says so in one line
  ("No smoke test yet") rather than being left out. `## Deploy` gets the
  platform, domain and production branch from round 11, and a
  `.env.example` is created if the stack uses environment variables.
- `DOCS/quality-targets.md`, if `bakjob-web` is enabled and a quality bar
  was chosen: the thresholds, WCAG level, meta title convention and JSON-LD
  schemas per page type.
- `DOCS/quality-targets.md`, if `bakjob-game` is enabled: target
  platforms, the frame rate from Performance (and the budget in ms), and
  memory/load-time budgets marked "not set yet" until they are.
- `DOCS/design/page-families.md`, if Page families = Yes: a heading per
  family with an empty route/arc table, for `content-page-family` to fill as
  pages get built.

## 4. Enable the plugins

For each plugin the user accepted in round 12, from inside `TARGET`:

```
claude plugin install <plugin>@bakjob --scope project
```

Project scope writes the plugin into `enabledPlugins` in
`TARGET/.claude/settings.json`, so it is active only in this project. The
install does **not** record where the marketplace comes from, so always add
`extraKnownMarketplaces` yourself; with it, anyone else who opens the project
is offered the same plugins. If the install command isn't available, write
both keys by hand:

```json
{
  "extraKnownMarketplaces": {
    "bakjob": { "source": { "source": "github", "repo": "Bakjob/Claude-settings" } }
  },
  "enabledPlugins": { "bakjob-core@bakjob": true, "bakjob-github@bakjob": true }
}
```

Tell the user to restart Claude Code (or run `/reload-plugins` if this
version has it) so the new plugins load.

**Pasted mode, or the user doesn't want plugins:** copy each accepted
plugin's `skills/*/` folders from `LIB/plugins/<plugin>/` into
`TARGET/.claude/skills/` and its `agents/*.md` into `TARGET/.claude/agents/`.
They need no editing, since they read the fact files; they just won't update
on their own.

## 5. Issue tracker variants

**Linear.** Enable `bakjob-linear`. In `CLAUDE.md`, keep the template's
tracker section with the workspace, team, project and issue prefix filled
in. If no `mcp__linear__*` tools
are available in this session, tell the user to connect it:
`claude mcp add --transport http linear https://mcp.linear.app/mcp`, then
`/mcp` to log in.

**GitHub Issues.** Enable `bakjob-github`. In `CLAUDE.md`, keep the tracker
section with the repo as `owner/name`; states are the
labels `in progress` and `in review`, and an issue is Done when closed. If
the user approved it in the summary, create the labels:

```
gh label create "in progress" --color FBCA04 --description "Someone is working on it" -R OWNER/REPO
gh label create "in review" --color 0E8A16 --description "Merged, waiting for a human to verify" -R OWNER/REPO
```

**Simple flow (any tracker).** If Flow = Simple, replace the "test loop" and
"In Review loop" parts with: an issue closes when its PR merges; PRs use
`Closes #N` (GitHub) or the tracker's equivalent.

**No tracker.** Replace the whole "Issue tracker" section with:

```markdown
## Todos

There is no issue tracker. Open work lives in `DOCS/status/todo.md` as a
checklist, newest at the top. Add a line when work is discovered, tick it
when it is verified (not just merged), and delete ticked lines once a
progress entry records them.
```

and create `DOCS/status/todo.md` with a heading and an empty list.

**Other.** Keep the generic tracker section with the tool's names filled in,
no workflow plugin. Add "a workflow plugin for TOOL" to open questions.

## 6. .claude/settings.json

Add to `TARGET/.claude/settings.json` (step 4 may already have created it).
Merge into what's there instead of replacing it. Build `permissions` from Permissions:

- **Generous:** allow the package manager's install/build/test/lint/format
  commands, `git status`, `git diff`, `git log`, `git add`, `git commit`,
  `git checkout`, `git switch`, `git branch`, plus `git push` and
  `gh pr create` if Autonomy allows pushing.
- **Standard:** allow only the read-only and build/test commands.
- **Careful:** allow nothing extra.
- Always deny `Bash(git push --force:*)`, `Bash(git push -f:*)`,
  `Read(./.env)` and `Read(./.env.*)`.

If Formatter = Yes, add a hook that formats each file Claude edits (needs
`jq`; check `command -v jq` first and tell the user if it's missing):

```json
{
  "permissions": {
    "allow": ["Bash(pnpm test:*)", "Bash(pnpm lint:*)"],
    "deny": ["Bash(git push --force:*)", "Read(./.env)"]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs -r pnpm exec prettier --write --ignore-unknown"
          }
        ]
      }
    ]
  }
}
```

Swap the formatter for the stack: `gdformat` (Godot, gdtoolkit), `dotnet
format` (Unity/C#), `cargo fmt` (Rust).

## 7. Git files

- `.gitignore` for the stack (node_modules, build output, `.env*`; Godot
  `.godot/`; Unity `Library/ Temp/ Obj/ Build/ Logs/ UserSettings/`; Rust
  `target/`; `.visual-check/` when `bakjob-web` is enabled). Merge with an
  existing one.
- If LFS = Yes: `.gitattributes` tracking the stack's binary asset types
  (images, audio, models, fonts). Check `git lfs version` first.
- If Host is set and there is no repo yet: `git init`. Creating a remote
  (`gh repo create`) only if the user approved it in the summary.
- If CI = GitHub Actions: `.github/workflows/ci.yml` running install, lint,
  typecheck, test and build from `running.md` on `pull_request`.
- First commit, in the commit language and style chosen, on the branch the
  git workflow says (main for the initial setup is fine). Push only if
  Autonomy allows it.
