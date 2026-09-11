# AGENTS.md

Vanilla JS Tetris (HTML5 Canvas). No dependencies, no build, no tests, no lint.

## Run

- No install/build needed. Open directly (`open index.html`) or serve statically: `python3 -m http.server 8000` then visit `http://localhost:8000`.
- There are no test, lint, or typecheck commands in this repo; do not invent package.json-based workflows.

## Architecture

- `game.js` holds all game logic; `index.html` and `style.css` are shell/theme only. Script loads at end of `<body>` because it queries DOM elements at top level; keep it that way.
- Board cells store `0` (empty) or a color index `1-7`. Cell value === piece type: `PIECES[type]` matrices are filled with their own type id, and `COLORS` is indexed the same way (`COLORS[1]` = I/cyan ... `COLORS[7]` = L/orange).
- `game.js` auto-initializes on load via the `init()` call at file end (`init` → `spawn` → `requestAnimationFrame(loop)`).

## Gotchas

- Canvas dimensions are hardcoded in `index.html`: `#board` is `COLS*BLOCK x ROWS*BLOCK` (300×600), `#next-canvas` is 120×120. If you change `COLS`, `ROWS`, or `BLOCK` in `game.js`, update the matching canvas `width`/`height` in `index.html` or rendering will clip.
- `LINE_SCORES` is indexed by lines cleared (`[0, 100, 300, 500, 800]`) and multiplied by current level; hard drop = 2 pts/cell, soft drop = 1 pt/row (see `game.js:108`, `hardDrop`, `softDrop`).
- Rotation uses simple transposition + row reverse (`rotateCW`) with ±0/±1/±2 wall kicks in `tryRotate`.
- Comments in code are English; README is Spanish.