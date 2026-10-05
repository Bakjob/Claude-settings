# Bootstrap interview

Rounds run in this order. Each "Q" is one question; options in brackets,
recommended first. Conditional questions are marked **if**. Every
`AskUserQuestion` question automatically gets an "Other" free-text option,
so options here only list the common answers.

After each round, keep a running answer sheet. Later rounds use it to pick
which questions to ask and which option to recommend.

---

## Round 1: The project (plain message, free text)

Ask together, in one message:

- What is the project called?
- Describe it in one or two sentences (the elevator pitch for `CLAUDE.md`).
- Confirm the target folder (show the resolved path).

## Round 2: Kind of project

- **Q Typ:** What kind of project is it?
  [Webbsajt (innehåll, marknad, portfolio) · Webbapp (inlogg, data, backend) · Spel · Annat (CLI, bibliotek, verktyg)]
- **Q Team:** Who works in it?
  [Bara jag · Jag + flera Claude-sessioner parallellt · Litet team · Öppen källkod]
- **Q Fas:** Where does it start from?
  [Helt nytt · Prototyp / game jam (snabbt, lite process) · Befintlig kod]

*"Prototyp / game jam" lowers the recommended process level in every later
round (lighter vault, no tracker, direct-on-main allowed).*

## Round 3: Stack

Skip any question pre-flight already answered from existing code; state the
detected answer instead.

- **if Webbsajt, Q Ramverk:** [Astro · Next.js (static export) · Vite + React · Ren HTML/CSS/JS]
- **if Webbapp, Q Ramverk:** [Next.js · SvelteKit · Vite + React + separat API · Annat]
- **if Webbapp, Q Data:** [Postgres (t.ex. Supabase/Neon) · SQLite · Firebase / hostad BaaS · Ingen databas än]
- **if Spel, Q Motor:** [Godot · Unity · Phaser / webbspel · Bevy / egen motor]
- **if Spel, Q Dimension:** [2D · 3D · Båda]
- **if Spel, Q Plattform:** [PC (Steam / itch.io) · Webb · Mobil · Konsol]
- **if any JS/TS stack, Q Pakethanterare:** [pnpm · npm · bun]
- **if JS/TS stack, Q Typning:** [TypeScript strikt · TypeScript · JavaScript]

## Round 4: Languages

- **Q Kod:** Language of code, identifiers and comments? [Engelska · Svenska]
- **Q Docs:** Language of docs and the vault? [Engelska · Svenska]
- **Q UI:** Language of user-facing text? [Engelska · Svenska · Båda (i18n från start)]
- **Q Commits:** Language of commit messages and PRs? [Engelska · Svenska]

## Round 5: Style

- **Q Chatt:** What language should Claude talk to you in? [Svenska · Engelska]
- **Q Tankstreck:** Keep the "never use em dashes" rule? [Behåll · Släpp]
- **Q Kodstil:** Code standard? [Mallens clean code-standard · Lättare (prototyp)]

## Round 6: Version control

- **Q Host:** [GitHub · GitLab · Bara lokal git · Ingen git ännu]
- **Q Branchar:** [Feature-branch + PR för allt utom triviala fixar · Direkt på main · Feature-branch utan PR (merge lokalt)]
- **Q Merge:** Who merges? [Alltid jag · Claude får merga när allt är grönt]
- **Q Autonomi:** What may Claude do on its own once a change is verified?
  [Committa, pusha och öppna PR utan att fråga · Committa själv, fråga före push · Fråga före varje commit]

## Round 7: Commits and PRs

- **Q Commitstil:** [Fri men tydlig (vad och varför) · Conventional Commits · Gitmoji]
- **Q PR-titlar:** [Gitmoji + issue-ID · Bara issue-ID · Ingen konvention]
- **if Spel or heavy assets, Q LFS:** Use Git LFS for binary assets? [Ja · Nej]
- **if Team is not "Bara jag", Q Testgren:** Want a disposable branch that combines every open PR for testing? [Ja · Nej]

## Round 8: Issue tracking

- **Q Tracker:** [GitHub Issues · Linear · Ingen tracker (todo-lista i vaulten) · Annan (Jira, Trello …)]
  *Recommend GitHub Issues when Host is GitHub, Linear if the user mentions
  using it, "Ingen" for prototypes.*
- **if a tracker, Q Flöde:** [Full loop: Done = verifierat av människa (In Review med checklista) · Enkel: issuen stängs vid merge]
- **if a tracker, Q Vad:** What gets an issue? [Allt arbete (features, buggar, förbättringar) · Bara större saker · Bara buggar]
- **if a tracker, Q Checklista:** Where does the manual test checklist go? [I issuen · I PR-beskrivningen]

Then, as a plain message, the identifiers the chosen tracker needs:

- **Linear:** workspace slug, team, project, issue prefix (e.g. `GAME`).
  Also check whether Linear MCP tools are available in this session.
- **GitHub Issues:** confirm `owner/repo` (read it from `git remote -v` if
  there is one); ask before creating the workflow labels.
- **Annan:** tool name, and how Claude should reach it (MCP, CLI, or not at
  all).

## Round 9: Quality

- **Q Tester:** [Unit + E2E · Bara unit · Bara build + lint + typecheck · Inget ännu]
- **if tests, Q Ramverk:** recommend per stack (Vitest + Playwright for web,
  GUT or gdUnit4 for Godot, Unity Test Framework for Unity, Vitest for Phaser,
  `cargo test` for Bevy) and offer the runner-up as the second option.
- **Q Formatter:** Run the formatter automatically after every edit Claude makes (hook)? [Ja · Nej]
- **Q CI:** [GitHub Actions på varje PR · Ingen CI ännu]
- **Q Smoke:** Is there (or will there soon be) a quick "does it still run" check? [Ja, beskriv kommandot · Inte än]

## Round 10: Docs and memory

- **Q Vault:** [Full: decisions + progress + status · Lätt: bara status + decisions · Ingen]
- **if vault, Q Mapp:** [vault/ · docs/]
- **if vault, Q Extra (multiSelect):** Extra folders?
  [game-design/ · design/ (UI, brand) · content/ · architecture/]
  *Pre-recommend game-design/ for Spel, design/ for Webbsajt.*
- **if vault, Q Obsidian:** Open the vault in Obsidian? [Ja · Nej]

## Round 11: How Claude works

- **Q Pushback:** How hard should Claude question requests?
  [Grilla mig: ifrågasätt allt som river upp fungerande system · Säg till vid tydliga problem · Bygg det jag säger]
- **Q Frågeformat:** Bigger question rounds later on?
  [Question desk-artifact (artifact-question-desk) · I terminalen]
- **Q Behörigheter:** [Generös: build, test, lint, git (utom push --force) utan att fråga · Standard · Försiktig: fråga för allt som skriver]
- **Q Deploy:**
  webb: [Vercel / Netlify / Cloudflare Pages · Statisk FTP-hosting · Egen server / container · Inte än];
  spel: [itch.io · Steam · Webb (statisk hosting) · Inte än]

## Round 12: Agents and skills

Build a proposal from the answer sheet using the table below, show it as a
table (name, what it does, why it fits), and ask
[Kör på förslaget · Justera (skriv vad som ska till eller bort)].

| Template | Recommend when |
|---|---|
| `git-checkpoint` | any git |
| `vault-update`, `new-decision`, agent `vault-scribe` | vault is Full or Lätt (`new-decision` needs decisions/) |
| `linear-dev-workflow` | Tracker = Linear |
| `github-issues-workflow` | Tracker = GitHub Issues |
| agent `architect` | not a prototype |
| agent `test-runner` | Tester is not "Inget ännu" |
| `smoke-test`, agent `smoke-test-runner` | Smoke = Ja **and** the command is known now |
| agent `config-value-auditor` | Spel, or any project with tuning values |
| agent `seo-a11y-auditor` | Webbsajt, or a public Webbapp |
| `design-taste-frontend` | Webbsajt or Webbapp with a designed UI |
| `redesign-skill` | Befintlig kod with a UI |
| `content-page-family` | Webbsajt with repeated page types |
| `static-site-deploy` | Deploy = Statisk FTP-hosting |
| `test-all-branches` | Testgren = Ja |
| `artifact-question-desk` | Frågeformat = question desk |

Only propose a template whose placeholders can be filled **now**. Anything
that fits but can't be filled yet goes into the report and the vault's
open questions as "add later, once X exists", not into `.claude/` with
placeholders left in.

## Round 13: Summary

See step 3 in [SKILL.md](SKILL.md).
