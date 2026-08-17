# Ideas / backlog

Candidate agents, skills, or template improvements not yet built. Not
permanent like `decisions/` — prune an entry once it's built (note it in
`progress/` instead) or once it's decided against (note why in
`decisions/` if the "no" is itself worth remembering).

- **`settings.json` / hooks starter template** — see open-questions.md for
  the undecided scope; if this gets built, it's a new top-level file/folder
  alongside `CLAUDE-template.md`.
- **A generic `code-review-checklist` skill** — several project-specific
  audit agents exist (`seo-a11y-auditor`, `config-value-auditor`); a
  lighter-weight generic pre-merge checklist skill (not agent) might be
  worth adding for projects too small to want a dedicated agent per concern.
- **CI/deploy templates beyond static hosting** — `static-site-deploy`
  assumes static export + FTP-style hosting. A second deploy skill template
  for a containerized/serverless deploy shape would cover more project
  types.
- **Example `.claude/agents` and `.claude/skills` folder layout note in the
  root README** — currently the README explains what to copy where in
  prose; a tiny ASCII tree showing the target project's resulting
  `.claude/` layout might make the copy step faster to follow.
