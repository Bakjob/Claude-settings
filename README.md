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

Run these inside Claude Code, in any session: the marketplace and
`bakjob-surdeg` are installed for your user, not for one project. Then open a
new (or existing) project folder and run:

```
/bakjob-surdeg:bootstrap
```

Bootstrap interviews you, writes the setup and enables the plugins it picked
for that project. To get a newer version later, refresh the marketplace and
update the plugin, then restart Claude Code:

```
/plugin marketplace update bakjob
/plugin update bakjob-surdeg@bakjob
```

You never need to add the marketplace again.

<details>
<summary>Without installing the plugin: copy the skills into the project</summary>

Copy this repo into a subfolder of your project (e.g. `_claude-settings/`)
and tell Claude: *"read `_claude-settings/BOOTSTRAP.md` and set up the
project"*. Bootstrap then **copies the skills and agents it picked into the
project's `.claude/`** and offers to delete the subfolder when it's done.
Nothing ends up outside the project folder, so the project works the same on
another machine with no plugin installed.

The same works from the installed plugin: in round 12 choose "Copy into the
project". The copies don't update on their own; run `/feed` to compare them
against a fresh copy of this library and overwrite only what you accept.
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

Later, two more commands from the same plugin (plain `/feed` and `/doctor` if
you copied the skills into the project):

- **`/bakjob-surdeg:feed`** changes part of the setup: switch tracker, add a
  deploy target, turn game tools on. It asks only the rounds you pick, with
  today's answers as the defaults, and shows the change before making it.
- **`/bakjob-surdeg:doctor`** checks the setup and offers fixes: missing
  facts the plugins need, plugins that should be on, a stale vault, issues
  stuck in progress.

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

See [`examples/`](examples/) for two complete results: a Godot game and an
Astro website.

```
your-project/
├── CLAUDE.md              how Claude works in this project
├── .claude/
│   └── settings.json      the plugins this project uses, permissions,
│                          optional formatter hook (with "copy into the
│                          project": skills/ and agents/ here too)
├── vault/                 hard rules, how to run it, decisions, progress, status
└── .gitignore, CI         if you asked for them
```

With the defaults, every change in your project then follows one loop:
**issue → branch → pull request → you merge → you verify → closed.** Claude
creates and claims the issue, opens the PR, and puts a checklist of what
needs your eyes on the issue. "Done" means you confirmed it works, not just
that it merged.

## The plugins

The `bakjob` marketplace has one plugin you install for yourself and five
that bootstrap turns on per project, so a game never loads web tools and a
website never loads game tools:

| Plugin | Installed | What it brings |
|---|---|---|
| `bakjob-surdeg` | once, by you | `bootstrap`, `feed`, `doctor` |
| `bakjob-core` | per project | git checkpoints, vault upkeep and decision records, smoke tests, a test runner, an architect for hard plans, a config-value auditor, the question desk artifact |
| `bakjob-github` | per project, GitHub Issues | the issue → PR → review loop with `gh` |
| `bakjob-linear` | per project, Linear | the same loop through Linear MCP |
| `bakjob-game` | per project, games | engine rules for Godot / Unity / Phaser / Bevy, a performance auditor against your frame budget, playtests that split provable checks from feel, an accessibility checklist, releases to itch.io and Steam |
| `bakjob-web` | per project, web | SEO/accessibility/performance audits, before/after screenshots of UI changes, frontend design, redesign audits, families of similar pages, deploys to Vercel / Netlify / Cloudflare Pages / containers / FTP |

None of them hold project-specific values. They read them from your
project's `CLAUDE.md` and `vault/` (commands from `vault/running.md`, rules
from `vault/hard-rules.md` …), so one improvement here reaches every project
on the next update. When a skill needs something that isn't written down yet, it asks once and
writes the answer where the next session will find it.
The full list of what lives where is in
[`project-facts.md`](plugins/bakjob-surdeg/skills/bootstrap/project-facts.md).

You can enable a plugin in a project by hand too:

```
claude plugin install bakjob-web@bakjob --scope project
```

## Contributing

Issues and pull requests are welcome. `CLAUDE.md` describes how changes are
made here (an issue per change, a PR per issue, how to verify), and
[`vault/`](vault/) records why the library looks the way it does.

## License

[MIT](LICENSE)
