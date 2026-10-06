---
name: game-release
description: Ship a build of a game - versioning and changelog, exporting per platform, uploading to itch.io with butler or to Steam with SteamPipe, web builds, and the checks before and after a release. Use when preparing a release, a demo or a playtest build, setting up release automation, or when an upload fails.
---

# Game release

## Project facts

Release targets and their identifiers are in the `## Deploy` section of
`DOCS/running.md` (`DOCS` is the docs folder the project's `CLAUDE.md` names,
usually `vault/`): the itch.io `user/game` and channel names, the Steam app
ID and depot IDs and where the SteamPipe build scripts live, the export
presets per platform, and the version scheme. If something is missing, ask
the user once and write it there. Credentials (butler API key, Steam login)
never go in the repo or in that file.

## Ground rules

- **Uploading is the user's call** unless `CLAUDE.md` says otherwise.
  Building and checking a release locally is always fine.
- **Never set a build live on Steam's default branch from a script.** Upload
  to a beta branch; the user sets it live in Steamworks.
- One version number per release, used everywhere: the game's own version
  display, the git tag, the upload, the changelog.

## Steps

1. **Version and changelog.** Bump the version (semantic: major.minor.patch,
   or the scheme `running.md` names), write the player-facing changelog from
   the merged issues since the last tag, and tag the commit:
   `git tag v1.4.0`.

2. **Export each platform** with the engine's export command (presets from
   `running.md`; see `engine-conventions` for the headless form). Use
   release, not debug, exports.

3. **Check the builds before uploading:** each one starts, reaches gameplay,
   saves and loads, and shows the right version. Run the smoke test against
   the exported build if the project supports it. Starting a build on a
   platform Claude can't run goes on the user's checklist.

4. **Upload.**
   - **itch.io (butler):** `butler login` once, then per channel
     `butler push build/windows user/game:windows --userversion 1.4.0`
     (likewise `linux`, `mac`, `html5`). Check with `butler status user/game`.
   - **Steam (SteamPipe):** keep the `app_build_<appid>.vdf` and depot
     scripts in the repo (no credentials in them), then
     `steamcmd +login <user> +run_app_build <path>/app_build_<appid>.vdf +quit`
     with `setlive` pointing at a beta branch. The user sets it live.
   - **Web builds:** a Godot web export with threads needs the
     `Cross-Origin-Opener-Policy: same-origin` and
     `Cross-Origin-Embedder-Policy: require-corp` headers (on itch.io, tick
     "SharedArrayBuffer support"), or export without threads. Test the
     uploaded page in a browser, not just the local file.

5. **After release:** check the store page shows the new version, download
   and start it once per platform (user checklist), post the changelog
   where the project announces releases, and record anything that went
   differently in `running.md`.

## Store page checklist (first release)

Capsule and header images in every size the store asks for (Steamworks and
itch.io list the current sizes; check there rather than from memory), at
least 5 screenshots from real gameplay, a trailer or GIF, a short and a long
description, tags, system requirements, the age rating questionnaire on
Steam, and a press kit page if the game will be pitched.
