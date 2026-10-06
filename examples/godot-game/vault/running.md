# Running Ember Hollow

Godot 4.5, GDScript. The project has no code yet; these commands apply once
it's scaffolded.

## Setup

1. Install Godot 4.5 (standard, not .NET) and put `godot` on `PATH`.
2. `git lfs install` once per machine, then clone.
3. `pip install "gdtoolkit==4.*"` for `gdformat` and `gdlint`.
4. Install GUT from the Asset Library into `addons/gut/`.
5. `godot --headless --path . --import` after cloning or adding assets.

## Dev

Open the project in the Godot editor and press F5, or
`godot --path .` to run the main scene.

## Build

Export presets live in `export_presets.cfg` (Windows, Linux, Web).

```
godot --headless --path . --export-release "Windows Desktop" build/windows/ember-hollow.exe
godot --headless --path . --export-release "Linux" build/linux/ember-hollow.x86_64
godot --headless --path . --export-release "Web" build/web/index.html
```

## Test

Unit tests with GUT in `tests/`:

```
godot --headless --path . -s addons/gut/gut_cmdln.gd -gexit
```

No E2E tests. CI runs lint, format check, import and the unit tests on every
PR (`.github/workflows/ci.yml`).

## Lint and format

```
gdformat .        # format (also runs after every edit Claude makes)
gdformat --check .
gdlint .
```

## Smoke test

No smoke test yet. Planned: a headless seeded run of one forest floor with a
random-input bot (see the `playtest` skill), added to this section once it
exists.

## Deploy

Releases go to itch.io with butler; Steam comes later.

- itch.io: `example/ember-hollow`, channels `windows`, `linux`, `html5`.
- Version scheme: semantic versions, tag `vX.Y.Z`, shown on the title screen
  from `Config.VERSION`.
- Web build: exported without threads, so no SharedArrayBuffer headers are
  needed.
