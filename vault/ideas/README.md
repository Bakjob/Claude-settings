# Ideas / backlog

Candidate agents, skills, or template improvements not yet built. Not
permanent like `decisions/` — prune an entry once it's built (note it in
`progress/` instead) or once it's decided against (note why in
`decisions/` if the "no" is itself worth remembering).

- **A generic `code-review-checklist` skill** — several project-specific
  audit agents exist (`seo-a11y-auditor`, `config-value-auditor`); a
  lighter-weight generic pre-merge checklist skill (not agent) might be
  worth adding for projects too small to want a dedicated agent per concern.
- **CI/deploy templates beyond static hosting** — `static-site-deploy`
  assumes static export + FTP-style hosting. A second deploy skill template
  for a containerized/serverless deploy shape would cover more project
  types.
- **`bakjob-game` plugin** — a game-dev pack: a `game-design/` vault
  seed with core loop / mechanics / tuning tables, a performance (frame
  budget) agent, a playtest skill splitting measurable checks from "feel"
  checkboxes, engine notes (Godot, Unity, Phaser). Bootstrap already asks
  for the engine, so the plugin slots straight into round 12 next to
  `bakjob-web`.
