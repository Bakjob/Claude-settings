# Hard rules

Short on purpose. Every rule here is enforced; breaking one is a bug.

- **Never commit to `main`.** Every change goes through a branch and a PR.
- **Never hand-edit `.godot/`, `*.import` or `*.uid` files.** Change import
  settings in the editor; commit `.import` and `.uid` files with their asset
  or script and move them together.
- **Scenes (`.tscn`) are editor-owned.** Changing a property value is fine;
  adding nodes or rewiring references is done in the editor or from code.
- **Binary assets go through Git LFS** (see `.gitattributes`).
- **No em dashes** in code, docs or in-game text.

**Config file:** `scripts/config.gd`, autoloaded as `Config`. Every
tunable value lives there: durations, cooldowns, speeds, damage, health,
spawn rates, drop chances, costs, limits.
