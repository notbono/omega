# OMEGA

A Claude.AI assisted JavaScript/HTML5 port, of a recreation of the arcade Omega Race game written in C++ with the Allegro library, circa mid-1990s. The game runs entirely in a single HTML file with no dependencies. You should be able to just open the file in a browser.

---

## Gameplay

You pilot a ship around a rectangular arena. Waves of enemy drones must be destroyed to advance to the next level. Shoot mines and enemies to destroy them.

### Enemy Types

| Type | Colour | Behaviour |
|------|--------|-----------|
| **Drone** | Blue hexagon | Patrols the outer zones at a fixed speed, bouncing off walls |
| **Badass** | Orange diamond | Like a drone, but fires bullets at the player and converts to a Freak after a timer expires |
| **Freak** | Magenta pentagon | Actively hunts the player when nearby; spirals erratically and drops mines when far away |

As levels progress, more drones spawn, more of them start as Badasses, and Freaks convert sooner.

### Mines

Freaks drop mines as they spiral around the arena. They can be destroyed with a single bullet for 500 points.

---

## Controls

| Key | Action |
|-----|--------|
| `↑` Up Arrow | Thrust forward |
| `←` / `→` Arrow | Rotate ship |
| `Space` | Fire |
| `P` | Pause / unpause |
| `Esc` | Quit to menu |

Movement uses Newtonian physics — thrust accelerates the ship and drag bleeds speed off gradually. Holding thrust while moving applies stronger drag, giving you better control at speed.

---

## Scoring

| Event | Points |
|-------|--------|
| Destroy a Drone | 1,000 |
| Destroy a Badass | 1,500 |
| Destroy a Freak | 2,500 |
| Destroy a Mine | 500 |

**Free lives** are awarded at 40,000, 150,000, and 250,000 points.

---

## Level Progression

- You start with 4 lives at Level 1.
- Clearing all drones advances you to the next level.
- Dying (collision with a drone, mine, or enemy bullet) costs one life and restarts the current level.
- Drone count increases each level, capped at 20.
- Drone speed, the number of Badasses/Freaks, and how quickly Badasses convert all scale with level number.
- Mines from the previous level persist between levels (except any in the ship's spawn zones, which are cleared).

---

## High Scores

The top 5 scores are saved to your browser's `localStorage` and persist between sessions. If you beat the 5th place score at game over, you'll be prompted to enter your 3-letter initials using arrow keys and spacebar.

---

## Technical Notes

- **Single file**: the entire game is `omega.html`. This can be run locally if desired.
- **Canvas rendering**: drawn with the HTML5 Canvas 2D API. Enemies and the ship are rendered procedurally (no sprite assets needed).
- **Physics timing**: three independent timers replicate the original Allegro interrupt timers:
  - `timer` — increments every second; controls freak conversion timing
  - `timer2` — increments every 100ms; gates fire rate
  - `timer3` — pulses every 25ms (40 Hz); gates physics updates
- **Collision**: all wall collisions (inner box and outer boundary) use penetration-depth resolution against the full 20×20 bounding box of each entity. Bullet wall-crossing uses previous/current position comparison to prevent tunnelling at high speed.
- **Browser support**: any modern browser with Canvas and `localStorage` support (Chrome, Firefox, Safari, Edge).

---

## Original Version

The original game was written in C++ targeting MS-DOS, using the [Allegro game library](https://liballeg.org/) for graphics, input, and timer interrupts. It used fixed-point arithmetic (`fix` type), palette-based 8-bit colour (a pure green phosphor palette), sprite assets loaded from a binary `.dat` datafile, and dirty-rectangle rendering optimised for the Pentium-era hardware of the time.

Claude.ai's assistance with this port (and this readme) scares me a little. This port preserves all game logic faithfully while replacing platform-specific subsystems with their browser equivalents.
