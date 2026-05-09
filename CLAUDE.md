# Platformer Prototype

A single-file HTML5 canvas platformer in the style of the original Super Mario Bros. The user is building it iteratively, starting with movement/jump feel before adding enemies or other systems.

## How to run

Just open `index.html` in a browser — no build step, no server, no dependencies. `open index.html` works on macOS.

## File layout

- `index.html` — everything: HTML shell, inline CSS, inline JS game loop. Keep it single-file until there's a real reason to split.
- `CLAUDE.md` — this file.

## Architecture (inside `index.html`)

The game is one IIFE inside a `<script>` tag. Top-to-bottom:

1. **Constants block** (`// ---------- Constants (tune these) ----------`) — all game-feel numbers live here. **Always tune feel by editing this block, not by sprinkling magic numbers elsewhere.**
2. **Level** — `LEVEL` is a string array using a tile legend: `.` empty, `#` solid, `=` one-way platform, `P` player spawn, `G` goal. Solids in the same row get merged horizontally at load time to reduce collision checks and visual seams.
3. **Player state** — single `player` object.
4. **Input** — `keydown`/`keyup` handlers; jump press sets `jumpBuffer` (used with coyote time).
5. **Collision** — `moveAxis(dx, dy)` resolves one axis at a time against merged solids; one-way platforms only collide when moving down and feet were above the platform top last frame.
6. **Update / render / loop** — fixed-ish `dt` clamped at 50 ms.

## What's implemented

- Acceleration-based horizontal movement with ground friction and a turn-around boost when reversing direction.
- Variable-height jump (release early = shorter hop) via three gravity values: `GRAVITY`, `FALL_GRAVITY`, `LOW_JUMP_GRAV`.
- Coyote time (`COYOTE_TIME`) and jump buffering (`JUMP_BUFFER`).
- Solid blocks and one-way (jump-through) platforms.
- Pits — falling off the bottom respawns at `P`.
- Goal flag (`G`) — touching it sets `player.won` and freezes the player.
- Camera follows player, clamped to level bounds. Parallax clouds.
- Debug overlay (backtick key) shows hitbox + position/velocity.

## What's NOT implemented yet (don't add unsolicited)

- Enemies, hazards beyond pits, projectiles.
- Coins / score / lives / timer.
- Animation frames (player is a static shape that just flips its eye).
- Sound.
- Multiple levels or a level-select.
- Power-ups, size changes, fireballs.
- Mobile/touch controls.
- A level editor or external level files (level is hardcoded as an array of strings).

The user explicitly said "don't worry about enemies yet" on the first turn — the current focus is **tightening movement and jump feel**.

## Controls

- `← →` or `A D` — move
- `Space`, `W`, or `↑` — jump (hold for higher)
- `R` — reset to spawn
- `` ` `` (backtick) — toggle debug overlay

## Tuning guidance

When the user reports something feeling off, the relevant constants are:

| Complaint | Knob(s) |
|---|---|
| "Feels sluggish to start moving" | `MOVE_ACCEL`, `AIR_ACCEL` |
| "Top speed too slow/fast" | `MAX_RUN_SPEED` |
| "Slides too much when I let go" | `GROUND_FRICTION` |
| "Hard to change direction" | `TURN_BOOST` |
| "Jump too floaty / too heavy" | `GRAVITY`, `FALL_GRAVITY` |
| "Can't get short hops" | `LOW_JUMP_GRAV` (raise it) |
| "Jump too low/high" | `JUMP_VELOCITY` |
| "Falls too fast at apex" | ratio of `FALL_GRAVITY` to `GRAVITY` |
| "Jumps off ledges feel unfair" | `COYOTE_TIME` |
| "Pressing jump just before landing doesn't register" | `JUMP_BUFFER` |

Defaults are tuned for a Mario-ish feel; the user may want something different. Ask before making sweeping changes — usually one or two constants is enough.

## Conventions

- Single file, no build step, no npm. Don't introduce a bundler or framework without explicit ask.
- Coordinates are in pixels; tile size is `TILE = 32`.
- Y grows downward (canvas convention).
- Collision resolves X then Y, separately, so corners feel right.
- Level edits go in the `LEVEL` array. New tile types need: a legend entry, parsing in the load loop, a render branch, and (if collidable) handling in `moveAxis`.
