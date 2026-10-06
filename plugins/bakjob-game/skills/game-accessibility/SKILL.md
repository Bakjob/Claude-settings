---
name: game-accessibility
description: Accessibility for games - remappable controls, alternatives to holding and rapid pressing, subtitles and captions, not relying on color alone, text and UI scale, screen shake, flashing and motion-sickness options, difficulty and assist options, visual cues for sound. Use while designing or building a feature with input, text, audio, camera or effects, and as an audit before a release.
---

# Game accessibility

Most accessibility work in games is cheap when it's planned with a feature
and expensive when it's retrofitted. Use this skill in both places: when a
feature is designed or built, check its part of the list; before a release,
audit the whole game against the project's target tier.

## Project facts

The target tier is in the `## Accessibility` part of
`DOCS/quality-targets.md` (`DOCS` is the docs folder the project's
`CLAUDE.md` names, usually `vault/`): **Basic**, **Intermediate** or
**Advanced**, following the Game Accessibility Guidelines
(gameaccessibilityguidelines.com), plus any platform requirement the game
must meet (Xbox Accessibility Guidelines for Xbox, CVAA for in-game chat
sold in the US). If it isn't set, use Basic and say so.

## The list

Each item is tagged with the lowest tier that expects it.

**Input**
- (Basic) Every action can be remapped, on keyboard and controller.
- (Basic) No action needs rapid repeated presses or a long hold without an
  alternative (toggle instead of hold, auto-repeat).
- (Basic) The game is playable with one input device; no required
  simultaneous presses across both sticks and triggers without an option.
- (Intermediate) Adjustable sensitivity, dead zones and aim assist; timing
  windows (QTEs, parries) that can be widened or turned off.

**Seeing**
- (Basic) Information never relies on color alone: shape, icon, pattern or
  text carries it too. A colorblind filter alone doesn't fix this.
- (Basic) Text is readable at the target screen and distance: a minimum
  size (around 28 px at 1080p on a TV, smaller on desktop), high contrast
  against its background.
- (Intermediate) UI and text scale; a high-contrast or clearer-background
  option for subtitles and HUD.
- (Advanced) Screen reader support for menus (the platform's or the engine's
  accessibility API), and audio description for cutscenes.

**Hearing**
- (Basic) Subtitles for all speech, on by default or offered at first
  launch, with a size option, a background, and the speaker's name.
- (Basic) Separate volume for music, effects and speech.
- (Intermediate) Captions for important sounds ("[footsteps behind you]"),
  and a visual cue for any sound the player must react to.

**Motion, flashing and comfort**
- (Basic) Screen shake, camera bob, motion blur and chromatic effects can be
  turned off or down.
- (Basic) Nothing flashes more than three times per second over a large
  area of the screen; a photosensitivity warning if anything comes close.
- (Intermediate) Field of view adjustable in first person; a fixed point
  (crosshair, frame) option for motion sickness.

**Thinking and pacing**
- (Basic) The game can be paused anywhere outside online play; no hard time
  limits on reading text or menus.
- (Basic) Clear, repeatable instructions: the controls and the current
  objective can be checked at any time.
- (Intermediate) Difficulty options that change specific things (damage
  taken, enemy speed, puzzle hints) rather than only one global setting.
- (Advanced) Assist modes: skip a section, invincibility, slow motion,
  without shaming the player for using them.

## Using it

- **When building a feature:** pick the items it touches (a new input, new
  UI text, a new effect, a timed challenge) and build them in. A new effect
  gets its toggle in the same PR; a new action is remappable from the start.
  Options go in the settings menu the project already has (see `CLAUDE.md`'s
  configurable options rule), not as one-off toggles.
- **As an audit:** go through the list up to the target tier. Check the
  code and settings for what can be checked (remapping, toggles, subtitle
  options, color-only signals in UI code); put what needs eyes (readability
  on a TV, flashing intensity, how it feels with assists on) on the issue
  as checkboxes, like `playtest` does.
- **Report** per area: met, missing, or needs a human check, with the file
  or screen involved.
