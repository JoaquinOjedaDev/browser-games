# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of self-contained browser games — each game is a single `.html` file with all logic, styles, and assets inlined. No build tools, no dependencies, no server required.

## Git & GitHub Workflow

This project uses https://github.com/JoaquinOjedaDev/browser-games. **After every meaningful unit of work — a new feature, a bug fix, any non-trivial change — commit and push immediately.** Never batch up multiple unrelated changes into one commit.

```bash
git add <files>
git commit -m "short imperative summary"
git push
```

Commit message rules: imperative mood, lowercase, no period. Describe *what* changed and *why* if it isn't obvious. The goal is that no work is ever lost and the history reads cleanly.

## Running / Testing

Open any `.html` file directly in a browser:

```bash
start tictactoe.html
start shooter.html
```

There are no build steps, package managers, or test suites.

## Code Architecture

Each game file follows the same pattern:

- **Single HTML file** — CSS in `<style>`, all JS in one `<script>` block
- **HTML5 Canvas** for rendering (`requestAnimationFrame` game loop)
- **No external libraries or assets** — all sprites are drawn with Canvas 2D API primitives (`arc`, `fillRect`, `lineTo`, etc.)
- **`localStorage`** for persistence (high scores)

### shooter.html — Top-Down Shooter

- **State machine**: `MENU → PLAYING → LEVEL_COMPLETE → GAME_OVER`
- **Game loop**: `loop(ts)` → `update(dt)` → `render()`, delta-time capped at 100ms
- **Entity arrays**: `bullets`, `eBullets`, `enemies`, `particles`, `floats`
- **Player object `P`**: position, angle (tracks mouse), HP, invincibility timer, shoot cooldown
- **Enemy types** (`EDEFS`): grunt, flanker, shooter, tank — each with distinct movement logic inside `updateEnemies()`
- **Level config** (`LEVELS` array): quota, spawn interval, allowed enemy types per level
- **Canvas scaling**: fixed 800×600 logical resolution, CSS-scaled to fit viewport via `resizeCanvas()`

### tictactoe.html — Tic Tac Toe

- Vanilla JS, DOM-based (no Canvas)
- Score state tracked in a plain `scores` object, reset only on page reload
