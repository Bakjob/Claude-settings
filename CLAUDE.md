# Claude-settings

*This file is for working on the library itself. If this repo was pasted
into another project to bootstrap it, ignore this file and follow
`BOOTSTRAP.md` instead.*

Jakob's library of reusable Claude Code configuration, shipped as the plugin
`bakjob-surdeg` (marketplace `bakjob`). Its `bootstrap` skill interviews
the user about a new project and generates that project's setup from
`templates/`. `README.md` explains the layout; `vault/` is the library's own
memory. Read `vault/status/current-status.md` before starting real work.

Talk to the user in the language they write in. **Everything in the repo is
English**, since it is public and meant for anyone: files, issues, PRs and
commit messages.

---

## Decisions

`vault/decisions/` holds the settled calls about how the library works (the
plugin layout, placeholders, how tracker skills are written). If a request
goes against one of them, say which decision and what it says, and ask
**"Are you sure?"** before building it. If the answer is yes, record a new
decision that supersedes the old one, as `vault/decisions/README.md`
describes.

---

## Issues: GitHub Issues on Bakjob/Claude-settings

**Every change goes through a GitHub issue**: new templates, reworks of
existing ones, bootstrap changes, docs. The mechanics (exact `gh` commands,
labels, closing) are in `.claude/skills/github-issues-workflow/`; use it.

- **Create the issue when the work starts**, not after. Write it so it reads
  well on its own: a short goal, a checklist of what's to be done, and when
  it's done. Link it to the overview issue it belongs to, if any.
- **Claim it before the first edit:** assignee and the `in progress` label in
  one command.
- **Name the issue in commits** (`#8: ...`) and in the PR title.
- **PRs use `Refs #N`, not `Closes #N`.** After merge the issue moves to
  `in review` with a checklist of what still needs the user's eyes (for
  example: run bootstrap on a real project). It closes when the user says it
  works. If everything was proven by the checks below, close it directly.
- **Sync after every merge**, whoever merged it.

---

## Git workflow: pull requests by default

- **All new work happens on a branch and lands through a pull request.** No
  direct commits to `main`, also not for small changes. Branch names:
  `feature/<slug>`, `fix/<slug>`, `docs/<slug>`, `chore/<slug>`.
- **Commit, push and open the PR without asking** once the change is
  verified (see below). Small, focused PRs; one issue per PR where possible.
- **PR titles:** a gitmoji for the kind of change (✨ feature, 🐛 fix,
  ♻️ rework, 📝 docs, 🔧 config) plus the issue number, e.g.
  `✨ #3 Bootstrap skill that interviews you`. The body says what changed and
  why, has `Refs #N`, and no test checklist (that goes on the issue).
- **Bump `version` in `.claude-plugin/plugin.json`** in every PR that changes
  what the plugin ships (`skills/`, `templates/`, the manifests). Installed
  copies are cached by version, so a change without a bump may never reach
  people who already installed it. Patch for fixes and wording, minor for new
  templates or questions, major when a generated project's layout changes.
- **Merging is always the user's click.** Say plainly when a PR is ready.
- Commit messages: direct, what changed and why, no filler.

---

## Verifying a change

Run these before calling a change done:

- `claude plugin validate .` passes.
- `claude --plugin-dir . plugin details bakjob-surdeg` lists only the
  skills that are meant to be live (today: `bootstrap`). A template in the
  root `skills/` or `agents/` by mistake shows up here.
- Templates under `templates/` keep their `[BRACKETED_PLACEHOLDERS]`;
  nothing under `skills/` or `.claude/` has any left.
- A template that ships HTML/JS (like `artifact-question-desk`) still passes
  `node --check` on its script.

---

## Keeping the library consistent

- A new or renamed template is listed in `README.md`, in bootstrap's round
  12 table (`skills/bootstrap/questions.md`), and in
  `vault/status/current-status.md`.
- After a real chunk of work, add a `vault/progress/` entry and update
  `vault/status/current-status.md`, as `vault/README.md` describes.
