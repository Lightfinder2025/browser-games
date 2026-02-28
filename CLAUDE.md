# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

A collection of single-file browser games. Each game is a self-contained `.html` file with no external dependencies — no libraries, no build step, no server required. Open any file directly in a browser.

## Running Games

```bash
open neon-siege.html
open tictactoe.html
```

## GitHub Workflow

This repo uses the GitHub CLI (`gh`). After every meaningful change, commit and push:

```bash
git add <file>
git commit -m "description"
git push
```

Remote: `https://github.com/Lightfinder2025/browser-games`

## Architecture Pattern

Every game follows the same single-file structure:

```
<head>   — CSS only (layout, screens, HUD, fonts)
<body>   — HTML overlay divs (start/gameover/victory screens) + main element (canvas or board)
<script> — All game logic, no external imports
```

**No external assets, libraries, images, or fonts are used.** All visuals are drawn with Canvas 2D `fillRect` (for canvas games) or pure HTML/CSS (for DOM-based games).

## Games

| File | Type | Notes |
|------|------|-------|
| `neon-siege.html` | Canvas 2D top-down shooter | 800×600 canvas, `requestAnimationFrame` loop, delta-time updates |
| `tictactoe.html` | DOM-based board game | Pure HTML/CSS/JS, no canvas |

## Neon Siege Architecture

- **Constants** at top: `W`, `H`, speeds, cooldowns, level configs (`LEVELS[]`), enemy type defs (`ENEMY_TYPES`)
- **State**: flat `let` vars (`score`, `currentLevel`, `killCount`) + `player` object + arrays (`bullets`, `enemies`, `particles`)
- **Loop**: `gameLoop(timestamp)` → `update(dt)` → `draw()` each frame
- **Collision**: simple AABB via `overlaps()` — rotation is visual only, hitboxes are axis-aligned
- **HUD**: HTML/CSS layer above the canvas, updated via DOM each frame in `updateHUD()`
- **Screens**: `.screen` divs toggled with `.visible` class; `gameState` string controls flow (`'start'|'playing'|'gameover'|'victory'`)
