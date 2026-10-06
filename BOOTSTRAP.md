# Bootstrap (for Claude)

If you are Claude and the user pasted this folder into their project and
asked you to set it up: read
`plugins/bakjob-surdeg/skills/bootstrap/SKILL.md` in this folder and follow
it. The library root (`LIB`) is the folder this file is in; the project being
set up is the folder around it (confirm with the user). Copy mode applies:
the skills and agents are copied from `LIB/plugins/` into the project's
`.claude/`, so the project depends on nothing outside its own folder.

Do not treat this folder's `vault/` as the project's vault. It is this
library's own history.
