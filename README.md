[README (3).md](https://github.com/user-attachments/files/32559268/README.3.md)
# Neon Nibbler

A self-contained neon maze-chase arcade game built with HTML, CSS, Canvas 2D, and the Web Audio API.

Neon Nibbler is an original game project. It does not use external images, fonts, libraries, audio files, backend services, or network requests.

## Play the game

[Play Game](https://ivanbarties-coder.github.io/pacman-style-game/neon-nibbler-extra-life.html)

> If the play link does not open, enable **GitHub Pages** for the repository using the `main` branch as the source. You can also open the [game HTML source](./neon-nibbler-extra-life.html) directly from the repository.

## Features

- Procedurally drawn 21 × 23 maze
- Canvas-rendered player, ghosts, pellets, power pellets, walls, overlays, and score popups
- Four ghost personalities:
  - **Murk** — direct pursuit
  - **Pip** — ambushes several tiles ahead of the player
  - **Zag** — flanks using the lead ghost's position
  - **Blip** — approaches cautiously and retreats to its corner when close
- Scatter and chase wave modes
- Power pellets that make ghosts vulnerable
- Escalating ghost-eating scores: 200, 400, 800, and 1,600 points
- Ten lives per new game
- One additional life awarded when advancing to the next level
- Level progression with increasing ghost and music tempo
- Corrected maze topology with all player pellets reachable
- Original synthesized chiptune music and sound effects generated with Web Audio
- Responsive layout with keyboard, touch-swipe, and on-screen D-pad controls
- Accessibility support including keyboard focus, live announcements, ARIA labels, and reduced-motion support
- No score or game-state persistence: all state is kept in memory for the current browser session

## Play online or locally

The game is a single HTML file. No build step or dependency installation is required.

### Option 1: Open directly

Open `neon-nibbler-extra-life.html` in a modern desktop or mobile browser.

### Option 2: Serve locally

Serving the file locally is useful when testing browser audio and mobile behaviour:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/neon-nibbler-extra-life.html
```

The browser may block audio until the first user interaction. Press **Start game** or a movement key to unlock audio.

## Controls

| Control | Action |
|---|---|
| Arrow keys | Move the player |
| `W`, `A`, `S`, `D` | Move the player |
| `P` or `Esc` | Pause or resume |
| Swipe on the game board | Move on touch devices |
| On-screen D-pad | Move on touch devices |
| Pause | Pause or resume the game |
| Sound on/off | Enable or mute all sound effects and music |
| Music on/off | Enable or disable background music |
| New game | Reset the current run |

## Gameplay

Clear every pellet in the maze without being caught.

- Small pellets are worth **10 points**.
- Power pellets are worth **50 points** and make eligible ghosts vulnerable.
- Vulnerable ghosts can be eaten for escalating points.
- Ghosts return to the house as eyes after being eaten.
- The player starts each new game with **10 lives**.
- When all pellets are cleared, the next level begins and the player receives **one additional life**.
- Lives are not capped, so successful level completion can increase the total above 10.
- Scatter windows become shorter and chase windows become longer as the level increases.
- Ghosts are slower than the player in normal pursuit, and slower again in tunnels. Frightened ghosts move at a reduced speed.

## Audio

All audio is generated at runtime with the Web Audio API.

The music system includes:

- An original C-major arcade theme
- A 150 BPM base tempo
- Title, gameplay, frightened, death, and level-clear tracks
- Look-ahead scheduling to keep musical events aligned
- Separate music gain routing
- Sound-effect ducking while effects play
- Mute and music-enable controls

No audio assets are downloaded or bundled.

## Technical overview

The project is intentionally implemented as one self-contained HTML file.

### Main layers

1. **Data**
   - Maze layout
   - Tile constants and directions
   - Ghost configurations
   - Music note arrays
2. **Simulation**
   - Player and ghost movement
   - Collision checks
   - Pellet consumption
   - Power-pellet frightened mode
   - Scatter/chase wave transitions
   - Level, life, and game-state management
3. **Rendering**
   - Canvas maze and entity drawing
   - Pellets and power pellets
   - Player animation
   - Ghost animation and frightened state
   - Floating score labels
4. **Shell**
   - HUD updates
   - Overlays and buttons
   - Keyboard and touch input
   - Main fixed-timestep loop
   - Visibility and audio handling

### Core dimensions

| Setting | Value |
|---|---:|
| Maze columns | 21 |
| Maze rows | 23 |
| Logical tile size | 24 px |
| Logical board size | 504 × 552 px |
| Simulation timestep | 1/120 s |
| Player speed | 96 px/s |
| Level 1 normal ghost speed | 72 px/s |
| Frightened ghost speed | 48 px/s |
| Eaten ghost speed | 192 px/s |
| Starting lives | 10 |
| Extra life on level advance | 1 |

Ghost chase speed increases gently by level but is capped below player speed. Ghosts are slowed in tunnel sections while the player is not slowed there.

## Project structure

```text
.
├── neon-nibbler-extra-life.html
└── README.md
```

The HTML file contains the markup, styles, game simulation, rendering code, input handlers, and audio engine.

## Development

Because the project has no dependencies, development is straightforward:

1. Edit `neon-nibbler-extra-life.html`.
2. Open it in a browser or serve it with a local HTTP server.
3. Test keyboard and touch input.
4. Test with sound enabled and disabled.
5. Test the start, pause, death, level-clear, next-level, and game-over flows.
6. Confirm that a completed level adds exactly one life before the next ready countdown.
7. Test with `prefers-reduced-motion` enabled.

A basic JavaScript syntax check can be performed with Node.js by extracting the script content first:

```bash
python3 - <<'PY'
import re

html = open("neon-nibbler-extra-life.html", encoding="utf-8").read()
scripts = re.findall(r"<script>([\\s\\S]*?)</script>", html)
open("/tmp/neon-nibbler.js", "w", encoding="utf-8").write("\\n".join(scripts))
PY

node --check /tmp/neon-nibbler.js
```

## Browser compatibility

Use a modern browser with support for:

- Canvas 2D
- Web Audio API
- `requestAnimationFrame`
- Pointer and touch events, where applicable
- CSS media queries

Desktop keyboard play and mobile touch play are both supported, but audio-unlock behaviour can vary by browser and device.

## Accessibility

The game includes:

- Keyboard-operable controls
- Visible focus styles
- ARIA labels for important controls
- A live region for significant game-state announcements
- Reduced-motion handling through `prefers-reduced-motion`
- Touch controls in addition to keyboard controls

Canvas gameplay remains primarily visual, so users who cannot perceive the canvas may need an alternative interface for full gameplay access.

## Known limitations

- The current build stores the best score only in memory; refreshing the page resets it.
- There is no pause-state persistence.
- Audio requires a user gesture in browsers that enforce autoplay restrictions.
- The project has no automated browser test suite yet.
- Visual balance and audio mix should be tested on real desktop and mobile devices.

## Original work

Neon Nibbler's artwork, game logic, music data, and synthesized sound design are original to this project and are not affiliated with any existing arcade property.

## License

No open-source license has been selected for this repository yet. Until a license is added, normal copyright restrictions apply and others should not assume they may reuse, modify, or redistribute the code.
