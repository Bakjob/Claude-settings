# Generating the project setup

Runs only after the user confirmed the summary. `LIB`, `TARGET` and the
answer sheet come from [SKILL.md](SKILL.md) and [questions.md](questions.md).
`DOCS` below means the docs folder the user chose (`vault/` or `docs/`).

Write everything in the languages the user picked: docs language for
`CLAUDE.md` and the vault, commit language for the first commit. Template
files are in English; translate them when the docs language is not English.

## 1. CLAUDE.md

Start from `LIB/templates/CLAUDE-template.md` and write `TARGET/CLAUDE.md`:

- Fill every placeholder from the answer sheet. `[HARD_RULES_FILE]` is
  `hard-rules.md`, `[RUNNING_FILE]` is `running.md` (both created in step 3).
- **Language rule:** state the four languages separately (code, docs, UI,
  commits) plus the chat language. Drop the em dash line if the user dropped
  that rule.
- **Issue tracker:** pick the variant in section 5 below.
- **Push back:** "Grilla mig" keeps the section as is. "Säg till vid tydliga
  problem" shortens it to the first paragraph. "Bygg det jag säger" replaces
  it with one line: flag a conflict with a decision or hard rule once, then
  build what was asked. Drop the `artifact-question-desk` sentence if the
  user chose terminal questions.
- **Clean code:** keep it for "Mallens standard"; for "Lättare", keep only
  the names, no-dead-code and comments-explain-why bullets.
- **Git workflow:** rewrite the bullets to match the answers, not the
  template's defaults: branch strategy, who merges, what Claude may do
  without asking, commit style, PR title convention. If there is no git,
  replace the section with one line saying so.
- **Configurable options:** keep only if the project will have a settings
  system; otherwise delete.
- **Agents and skills:** list exactly what step 4 installs, one line each.
- Delete the template's "Filling in this template" section.

## 2. Vault

Skip if Vault = Ingen (then `DOCS` is just a folder for `hard-rules.md` and
`running.md`, default `docs/`).

Copy `LIB/templates/vault/` to `TARGET/DOCS`, fill its placeholders, then:

- **Lätt:** delete `progress/` mentions from `README.md` and don't create the
  folder.
- **Full:** create `progress/YYYY-MM-DD-bootstrap.md` (today's date) with the
  whole answer sheet as a table, so a later session can see why the setup
  looks the way it does.
- Each extra folder gets a `README.md` saying what belongs there. For
  `game-design/`, seed sections: core loop, mechanics, progression, tuning
  values (pointing at the config file once it exists).
- `status/current-status.md`: "Bootstrapped on YYYY-MM-DD, no code yet" (or
  what pre-flight found in existing code).
- `status/open-questions.md`: every template that fits but couldn't be
  filled yet, as "add X once Y exists".

## 3. Hard rules and running file

- `DOCS/hard-rules.md`: short. Only rules that follow directly from the
  answers (languages, em dashes, commit style, "never commit to main" if
  feature branches were chosen) plus stack-specific ones that are always
  true, for example for Godot "never hand-edit `.import` files or `.godot/`",
  for Unity "never edit `.meta` files by hand; keep them committed".
- `DOCS/running.md`: install, dev, build, test, lint and format commands for
  the stack and package manager. For a project with no code yet, list the
  commands the chosen stack will use and say they apply once it is
  scaffolded.

## 4. Agents and skills

For each template the user accepted in round 12:

- Copy `LIB/templates/agents/<name>.md` to `TARGET/.claude/agents/`, or
  `LIB/templates/skills/<name>/` (the whole folder) to
  `TARGET/.claude/skills/<name>/`.
- Fill every placeholder. Remove the italic `*Template — fill in ...*`
  footer and any "*(Document this project's ... here)*" hints you filled.
- If a placeholder can't be filled honestly, don't invent a value: drop the
  template and move it to open questions instead.

## 5. Issue tracker variants

**Linear.** Install `linear-dev-workflow`. In `CLAUDE.md`, keep the template's
tracker section with Linear's names filled in. If no `mcp__linear__*` tools
are available in this session, tell the user to connect it:
`claude mcp add --transport http linear https://mcp.linear.app/mcp`, then
`/mcp` to log in.

**GitHub Issues.** Install `github-issues-workflow` with `[REPO]` filled. In
`CLAUDE.md`, keep the tracker section with GitHub's names; states are the
labels `in progress` and `in review`, and an issue is Done when closed. If
the user approved it in the summary, create the labels:

```
gh label create "in progress" --color FBCA04 --description "Someone is working on it" -R OWNER/REPO
gh label create "in review" --color 0E8A16 --description "Merged, waiting for a human to verify" -R OWNER/REPO
```

**Simple flow (any tracker).** If Flöde = Enkel, replace the "test loop" and
"In Review loop" parts with: an issue closes when its PR merges; PRs use
`Closes #N` (GitHub) or the tracker's equivalent.

**Ingen tracker.** Replace the whole "Issue tracker" section with:

```markdown
## Todos

There is no issue tracker. Open work lives in `DOCS/status/todo.md` as a
checklist, newest at the top. Add a line when work is discovered, tick it
when it is verified (not just merged), and delete ticked lines once a
progress entry records them.
```

and create `DOCS/status/todo.md` with a heading and an empty list.

**Annan.** Keep the generic tracker section with the tool's names filled in,
no workflow skill. Add "a workflow skill for TOOL" to open questions.

## 6. .claude/settings.json

Write `TARGET/.claude/settings.json`. Merge into an existing one instead of
replacing it. Build `permissions` from Behörigheter:

- **Generös:** allow the package manager's install/build/test/lint/format
  commands, `git status`, `git diff`, `git log`, `git add`, `git commit`,
  `git checkout`, `git switch`, `git branch`, plus `git push` and
  `gh pr create` if Autonomi allows pushing.
- **Standard:** allow only the read-only and build/test commands.
- **Försiktig:** allow nothing extra.
- Always deny `Bash(git push --force:*)`, `Bash(git push -f:*)`,
  `Read(./.env)` and `Read(./.env.*)`.

If Formatter = Ja, add a hook that formats each file Claude edits (needs
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
  `target/`). Merge with an existing one.
- If LFS = Ja: `.gitattributes` tracking the stack's binary asset types
  (images, audio, models, fonts). Check `git lfs version` first.
- If Host is set and there is no repo yet: `git init`. Creating a remote
  (`gh repo create`) only if the user approved it in the summary.
- If CI = GitHub Actions: `.github/workflows/ci.yml` running install, lint,
  typecheck, test and build from `running.md` on `pull_request`.
- First commit, in the commit language and style chosen, on the branch the
  git workflow says (main for the initial setup is fine). Push only if
  Autonomi allows it.
