# bakjob-surdeg

A sourdough starter for your projects: a
[Claude Code](https://claude.com/claude-code) plugin that sets up a new
project for working with Claude. Instead of guessing, it **interviews you
first**: stack, languages, version control, issue tracking, testing, docs,
and how much Claude may do on its own. Then it writes a setup that matches
your answers.

Built for web and game projects, usable for anything.

## Install

```
/plugin marketplace add Bakjob/Claude-settings
/plugin install bakjob-surdeg@bakjob
```

Run these inside Claude Code, in any session: marketplaces and plugins are
installed for your user, not for one project. Then open a new (or existing)
project folder and run:

```
/bakjob-surdeg:bootstrap
```

To get a newer version later, refresh the marketplace and update the plugin,
then restart Claude Code:

```
/plugin marketplace update bakjob
/plugin update bakjob-surdeg@bakjob
```

You never need to add the marketplace again.

<details>
<summary>Without installing the plugin</summary>

Copy this repo into a subfolder of your project (e.g. `_claude-settings/`)
and tell Claude: *"read `_claude-settings/BOOTSTRAP.md` and set up the
project"*. Claude offers to delete the folder when it's done.
</details>

## How it works

1. **Looks at the folder.** Empty or existing code, git, remote. The stack is
   detected from existing code instead of asked.
2. **Asks you, in 13 short rounds.** Multiple choice, the recommended answer
   first. Say "use the defaults for the rest" at any point.
3. **Shows a summary** of every answer, every file it will write and anything
   it will do outside your machine (like creating GitHub labels). Nothing is
   written before you say yes.
4. **Generates and checks** the setup: no leftover placeholders, valid JSON.

What it asks about:

| | |
|---|---|
| **Project** | name, pitch, website / web app / game / other, solo or team |
| **Stack** | framework, or engine, 2D/3D and platform for games; package manager |
| **Language** | for code, docs, UI and commits separately; the language Claude talks to you in |
| **Version control** | GitHub / GitLab / local, branches + PRs or straight to main, who merges, what Claude may commit and push alone, commit style, Git LFS |
| **Issues** | GitHub Issues, Linear, or none; full review loop or close on merge |
| **Quality** | unit / E2E tests, formatter hook, CI, smoke test |
| **Docs** | a project vault (decisions, progress log, status), or none |
| **Working style** | how hard Claude should push back, permissions, deploy target |

## What you get

```
your-project/
├── CLAUDE.md              how Claude works in this project
├── .claude/
│   ├── settings.json      permissions, optional formatter hook
│   ├── agents/            the agents that fit your answers
│   └── skills/            the skills that fit your answers
├── vault/                 hard rules, how to run it, decisions, progress, status
└── .gitignore, CI         if you asked for them
```

With the defaults, every change in your project then follows one loop:
**issue → branch → pull request → you merge → you verify → closed.** Claude
creates and claims the issue, opens the PR, and puts a checklist of what
needs your eyes on the issue. "Done" means you confirmed it works, not just
that it merged.

## What's in the library

The templates bootstrap picks from live in [`templates/`](templates/):

- **Workflow:** `git-checkpoint`, `github-issues-workflow`,
  `linear-dev-workflow`, `test-all-branches`
- **Project memory:** `vault-update`, `new-decision`, agent `vault-scribe`,
  a vault skeleton
- **Quality:** agents `test-runner`, `smoke-test-runner`, `architect`,
  `config-value-auditor`, `seo-a11y-auditor`, skill `smoke-test`
- **Web:** `design-taste-frontend`, `redesign-skill`, `content-page-family`,
  `static-site-deploy`
- **Asking you things:** `artifact-question-desk`
- **`CLAUDE-template.md`**, the base for each project's `CLAUDE.md`

You can also copy any of them by hand into `.claude/agents/` or
`.claude/skills/` and fill in the `[BRACKETED_PLACEHOLDERS]`.

## Contributing

Issues and pull requests are welcome. `CLAUDE.md` describes how changes are
made here (an issue per change, a PR per issue, how to verify), and
[`vault/`](vault/) records why the library looks the way it does.

## License

[MIT](LICENSE)
