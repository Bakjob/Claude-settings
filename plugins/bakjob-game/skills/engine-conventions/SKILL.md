---
name: engine-conventions
description: Rules for working inside a game engine project (Godot, Unity, Phaser, Bevy) - which files Claude must never hand-edit, which are generated, how scenes, prefabs and assets are organized, how to run the game and its tests headless, and how to keep engine metadata intact in git. Use before editing scenes, assets, project settings or engine metadata, when adding or moving assets, and when setting up a headless run or a test runner.
---

# Engine conventions

Most damage Claude can do to a game project isn't in the code: it's a
hand-edited scene that the editor can no longer open, a moved asset that lost
its metadata, or a generated file committed or deleted. This skill is the
per-engine list of what to touch and what not to.

## Project facts

The engine and its version are in the first paragraph of the project's
`CLAUDE.md`; project-specific additions to the rules below are in
`DOCS/hard-rules.md`, and the run, test and export commands in
`DOCS/running.md` (`DOCS` is the docs folder `CLAUDE.md` names, usually
`vault/`). If the engine version isn't written down, read it from the project
file (`project.godot`, `ProjectSettings/ProjectVersion.txt`, `package.json`,
`Cargo.toml`) and add it to `CLAUDE.md`.

## General rules

- **Moving or renaming an asset moves its metadata with it.** Do it in one
  step (with `git mv` for both files), or do it in the editor. Never leave
  an orphaned metadata file or an asset without one.
- **Scene and prefab files are editor-owned.** Small, obvious property
  changes in a text scene are fine; restructuring one by hand (adding nodes,
  rewiring references, changing IDs) is not. Describe the change and let the
  user make it in the editor, or generate the structure from code.
- **Binary assets** (textures, audio, models) go through Git LFS when the
  project uses it (`.gitattributes`). Check before adding a large file.
- **Gameplay numbers live in the config file** named in `hard-rules.md`, not
  in scenes or scripts (the `config-value-auditor` agent checks this).

## Godot (4.x)

- Never edit or commit `.godot/` (the editor's cache, regenerated on open).
- Never hand-edit `*.import` files; change import settings in the editor's
  Import dock. Commit them, they hold the settings.
- Commit `*.uid` files next to their scripts and resources (Godot 4.4+) and
  move them together; they keep references stable.
- `.tscn` / `.tres` are text: changing a property value is fine, but keep
  `ext_resource` IDs, `uid=` values and `load_steps` consistent, or open the
  scene in the editor and save it after.
- Autoloads and input actions live in `project.godot`; prefer adding them
  through Project Settings, and keep that file's sections intact.
- Headless: `godot --headless --path . --import` once after adding assets;
  run a script with `godot --headless --path . --script res://path/to/script.gd`;
  export with `godot --headless --path . --export-release "<preset>" <output>`.
- Tests: GUT (`godot --headless -s addons/gut/gut_cmdln.gd -gexit`) or
  gdUnit4; use whatever is in `addons/` already.

## Unity

- Every asset has a `.meta` file holding its GUID; references break without
  it. Never delete or regenerate `.meta` files, always commit them, and move
  them with their asset.
- Asset serialization must be **Force Text** (Project Settings → Editor) so
  scenes and prefabs diff and merge; set up UnityYAMLMerge as the merge tool
  for `.unity` and `.prefab`.
- Never commit `Library/`, `Temp/`, `Obj/`, `Build/`, `Logs/`,
  `UserSettings/`.
- Don't hand-edit scene or prefab YAML beyond a single field; `fileID`
  references are easy to break silently.
- Headless tests: `Unity -batchmode -projectPath . -runTests -testPlatform EditMode -testResults results.xml`
  (no `-quit` together with `-runTests`); builds via `-executeMethod` on a
  build script in `Assets/Editor/`.

## Phaser (web)

- It's a web project: the package manager, bundler (usually Vite) and
  `running.md` commands apply as for any web app.
- Static assets live in `public/assets/` (served as-is); keep keys in one
  place (an asset manifest or a preload scene) rather than strings spread
  across scenes.
- Keep game rules out of `Phaser.Scene` classes where possible, so they can
  be unit-tested with Vitest without a canvas; for a headless simulation use
  `type: Phaser.HEADLESS`.

## Bevy

- Asset files live in `assets/`; paths in code are relative to it.
- Fast iteration: `cargo run --features bevy/dynamic_linking` in dev only;
  never ship with it.
- Headless runs and tests use `MinimalPlugins` (plus the systems under
  test) instead of `DefaultPlugins`, so no window or GPU is needed.
- Keep systems small and data in components/resources; tuning values in a
  config resource loaded from one file.
