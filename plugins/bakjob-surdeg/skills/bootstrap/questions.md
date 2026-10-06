# Bootstrap interview

Rounds run in this order. Each "Q" is one question with its short header;
options in brackets, recommended first. Conditional questions are marked
**if**. Every `AskUserQuestion` question automatically gets an "Other"
free-text option, so options here only list the common answers.

This catalogue is written in English. Ask in the language the user writes
in, translating questions and option labels as you go; the answer sheet and
[generate.md](generate.md) refer to the English labels below.

After each round, keep a running answer sheet. Later rounds use it to pick
which questions to ask and which option to recommend.

---

## Round 1: The project (plain message, free text)

Ask together, in one message:

- What is the project called?
- Describe it in one or two sentences (the elevator pitch for `CLAUDE.md`).
- Confirm the target folder (show the resolved path).

## Round 2: Kind of project

- **Q Kind:** What kind of project is it?
  [Website (content, marketing, portfolio) · Web app (login, data, backend) · Game · Other (CLI, library, tool)]
- **Q Team:** Who works in it?
  [Just me · Me + several parallel Claude sessions · Small team · Open source]
- **Q Phase:** Where does it start from?
  [Brand new · Prototype / game jam (fast, little process) · Existing code]

*"Prototype / game jam" lowers the recommended process level in every later
round (light vault, no tracker, straight to main allowed).*

## Round 3: Stack

Skip any question pre-flight already answered from existing code; state the
detected answer instead.

- **if Website, Q Framework:** [Astro · Next.js (static export) · Vite + React · Plain HTML/CSS/JS]
- **if Web app, Q Framework:** [Next.js · SvelteKit · Vite + React + separate API · Other]
- **if Web app, Q Data:** [Postgres (e.g. Supabase/Neon) · SQLite · Firebase / hosted BaaS · No database yet]
- **if Game, Q Engine:** [Godot · Unity · Phaser / web game · Bevy / own engine]
- **if Game, Q Dimension:** [2D · 3D · Both]
- **if Game, Q Platform:** [PC (Steam / itch.io) · Web · Mobile · Console]
- **if any JS/TS stack, Q Package manager:** [pnpm · npm · bun]
- **if JS/TS stack, Q Typing:** [Strict TypeScript · TypeScript · JavaScript]
- **if Website, Q Page families:** Does the site have repeated page types (case studies, products, docs per API)? [Yes · No]

## Round 4: Languages

- **Q Code:** Language of code, identifiers and comments? [English · the user's language]
- **Q Docs:** Language of docs and the vault? [English · the user's language]
- **Q UI:** Language of user-facing text? [English · the user's language · Both (i18n from the start)]
- **Q Commits:** Language of commit messages, issues and PRs? [English · the user's language]

*"The user's language" means the language they write to Claude in, named
explicitly in the option (e.g. "Swedish"). If they write in English, offer
English plus "Other".*

## Round 5: Style

- **Q Chat:** What language should Claude talk to you in? [the user's language · English]
- **Q Em dashes:** Keep the "never use em dashes" rule? [Keep · Drop]
- **Q Code style:** [The template's clean code standard · Lighter (prototype)]

## Round 6: Version control

- **Q Host:** [GitHub · GitLab · Local git only · No git yet]
- **Q Branches:** [Feature branch + PR for everything but trivial fixes · Straight to main · Feature branch without PR (merge locally)]
- **Q Merge:** Who merges? [Always me · Claude may merge when everything is green]
- **Q Autonomy:** What may Claude do on its own once a change is verified?
  [Commit, push and open the PR without asking · Commit on its own, ask before pushing · Ask before every commit]

## Round 7: Commits and PRs

- **Q Commit style:** [Free but clear (what and why) · Conventional Commits · Gitmoji]
- **Q PR titles:** [Gitmoji + issue ID · Issue ID only · No convention]
- **if Game or heavy assets, Q LFS:** Use Git LFS for binary assets? [Yes · No]
- **if Team is not "Just me", Q Test branch:** Want a disposable branch that combines every open PR for testing? [Yes · No]

## Round 8: Issue tracking

- **Q Tracker:** [GitHub Issues · Linear · No tracker (todo list in the vault) · Other (Jira, Trello …)]
  *Recommend GitHub Issues when Host is GitHub, Linear if the user mentions
  using it, "No tracker" for prototypes.*
- **if a tracker, Q Flow:** [Full loop: Done = verified by a human (In Review with a checklist) · Simple: the issue closes on merge]
- **if a tracker, Q Scope:** What gets an issue? [All work (features, bugs, improvements) · Only bigger things · Only bugs]
- **if a tracker, Q Checklist:** Where does the manual test checklist go? [On the issue · In the PR description]

Then, as a plain message, the identifiers the chosen tracker needs:

- **Linear:** workspace slug, team, project, issue prefix (e.g. `GAME`).
  Also check whether Linear MCP tools are available in this session.
- **GitHub Issues:** confirm `owner/repo` (read it from `git remote -v` if
  there is one); ask before creating the workflow labels.
- **Other:** tool name, and how Claude should reach it (MCP, CLI, or not at
  all).

## Round 9: Quality

- **Q Tests:** [Unit + E2E · Unit only · Build + lint + typecheck only · Nothing yet]
- **if tests, Q Test framework:** recommend per stack (Vitest + Playwright for
  web, GUT or gdUnit4 for Godot, Unity Test Framework for Unity, Vitest for
  Phaser, `cargo test` for Bevy) and offer the runner-up as the second option.
- **Q Formatter:** Run the formatter automatically after every edit Claude makes (hook)? [Yes · No]
- **Q CI:** [GitHub Actions on every PR · No CI yet]
- **Q Smoke test:** Is there (or will there soon be) a quick "does it still run" check? [Yes, describe the command · Not yet]
- **if Game, Q Performance:** Target frame rate on the main platform?
  [60 FPS on a mid-range PC · 30 FPS on mobile · 120+ FPS (fast-paced) · Not decided yet]
- **if Game or Web app, Q Config file:** Keep every tunable value (durations, thresholds, costs, limits) in one config file?
  [Yes, at the path you suggest for the stack · No]
  *Suggest a path per stack, e.g. `src/config.ts`, a Godot autoload
  `scripts/config.gd`, a Unity ScriptableObject.*

## Round 10: Docs and memory

- **Q Vault:** [Full: decisions + progress + status · Light: status + decisions only · None]
- **if vault, Q Folder:** [vault/ · docs/]
- **if vault, Q Extra folders (multiSelect):**
  [game-design/ · design/ (UI, brand) · content/ · architecture/]
  *Pre-recommend game-design/ for Game, design/ for Website.*
- **if vault, Q Obsidian:** Open the vault in Obsidian? [Yes · No]

## Round 11: How Claude works

- **Q Push back:** How hard should Claude question requests?
  [Grill me: question anything that tears up working systems · Flag clear problems · Build what I say]
- **Q Question format:** Bigger question rounds later on?
  [Question desk artifact (artifact-question-desk) · In the terminal]
- **Q Permissions:** [Generous: build, test, lint, git (except push --force) without asking · Standard · Careful: ask for anything that writes]
- **Q Deploy:**
  web: [Vercel / Netlify / Cloudflare Pages · Static FTP hosting · Own server / container · Not yet];
  game: [itch.io · Steam · Web (static hosting) · Not yet]
- **if Website or public Web app, Q Quality bar:** [Lighthouse 90+ and WCAG 2.2 AA · Stricter (95+, AAA where practical) · Not yet]

If a deploy target was chosen, ask as a plain message for the domain and
anything the host needs (FTP target folder, project name on the platform).

## Round 12: Plugins

The everyday skills and agents come as plugins from the `bakjob`
marketplace. Build a proposal from the answer sheet, show it as a table
(plugin, what it brings, why it fits), and ask
[Go with the proposal · Adjust (say what to add or remove)].

| Plugin | Brings | Recommend when |
|---|---|---|
| `bakjob-core` | `git-checkpoint`, `vault-update`, `new-decision`, `smoke-test`, `test-all-branches`, `artifact-question-desk`; agents `architect`, `test-runner`, `vault-scribe`, `smoke-test-runner`, `config-value-auditor` | almost always; skip only for a throwaway prototype with no git and no vault |
| `bakjob-github` | `github-issues-workflow` | Tracker = GitHub Issues |
| `bakjob-linear` | `linear-dev-workflow` | Tracker = Linear |
| `bakjob-game` | `engine-conventions`, `playtest`, `game-release`; agent `performance-auditor` | Game |
| `bakjob-web` | `design-taste-frontend`, `redesign-skill`, `content-page-family`, `static-site-deploy`, `web-deploy`, `visual-check`; agent `seo-a11y-auditor` | Website or Web app |

Plugins are enabled per project, so a game project never loads the web
tools (and a website never loads the game ones). Facts a plugin needs that can't be known yet (a smoke test command
before there is code) are not invented: they go into the vault's open
questions, and the skill asks for them the first time it's used.

## Round 13: Summary

See step 3 in [SKILL.md](SKILL.md).
