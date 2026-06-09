# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

No build step, no dependencies. Open directly or serve statically:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Architecture

Three files, no framework:

- **`index.html`** — DOM structure. Two `<canvas>` elements: `#board` (300×600 px, the play field) and `#next-canvas` (120×120 px, the preview). The overlay `#overlay` handles both PAUSE and GAME OVER states via `.hidden`.
- **`style.css`** — Dark/retro theme. No dynamic classes beyond toggling `.hidden` on the overlay.
- **`game.js`** — All game logic (~300 lines, `'use strict'`, no modules).

### Key data structures in `game.js`

- **`board`** — `ROWS × COLS` (20×10) 2-D array; `0` = empty, `1–7` = piece color index.
- **`current` / `next`** — piece objects `{ type, shape, x, y }` where `shape` is a 2-D matrix of color indices.
- **`PIECES`** — piece shapes indexed 1–7 (index 0 is null). Rotation uses transpose + row-reverse (`rotateCW`).

### Game loop

`init()` → `spawn()` → `requestAnimationFrame(loop)`. The loop accumulates `dropAccum` and drops the piece when it exceeds `dropInterval`. Locking a piece calls `merge()` → `clearLines()` → `spawn()`; if the spawned piece immediately collides, `endGame()` fires.

### Tunable constants

| Constant | Default | Effect |
|---|---|---|
| `COLS` / `ROWS` | 10 / 20 | Board dimensions — update canvas `width`/`height` in HTML to match (`COLS×BLOCK` / `ROWS×BLOCK`) |
| `BLOCK` | 30 | Pixel size per cell |
| `COLORS` | 7 hues | Color per piece type |
| `LINE_SCORES` | `[0,100,300,500,800]` | Points for 1–4 simultaneous line clears, multiplied by current level |
| `dropInterval` | 1000 ms | Starting fall speed; auto-recalculated as `max(100, 1000 − (level−1)×90)` |
