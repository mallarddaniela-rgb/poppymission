# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Poppymission is a single-file PWA (`index.html`) — no build step, no dependencies, no package manager. Everything lives in one HTML file with inline CSS and inline `<script>`. The only external asset is `apple-touch-icon.png` (180pt @2x Retina, generated via Swift/AppKit).

## Running the app

Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

There are no tests, no linter, and no CI config.

## Architecture

The app is a 4-screen flow, all in a single `#app` container. Screens are absolutely positioned and shown/hidden via CSS classes (`.active`, `.exit-left`); transitions are CSS-driven.

**Screen flow:**
1. `#screen-home` — bouncing 💩 button + settings gear
2. `#screen-bristol` — Bristol Stool Scale type picker (types 1–7, built from the `BRISTOL` array)
3. `#screen-questions` — two sequential yes/no questions ("Clean wipe?", "Great poop?"), injected dynamically into `#question-wrapper`
4. `#screen-confirm` — flush animation, auto-returns home after 2.4 s

**State:** A single `currentEntry` object accumulates `{ bristolType, cleanWipe, greatPoop }` as the user progresses, then gets a `date`/`time` stamp before being pushed into `localStorage`.

**Persistence:** All entries are stored as a JSON array in `localStorage` under the key `poppymission_logs`. Helper functions: `loadLogs()`, `saveLogs()`, `addEntry()`.

**Settings drawer:** Rendered outside `#app` as a fixed overlay (`#settings-overlay`). Handles entry count display and CSV export.

## Generating the app icon

The `apple-touch-icon.png` is created with a Swift script (requires macOS). Run this if the icon needs to be regenerated:

```bash
swift /tmp/gen_icon.swift   # see script inline in git history / conversation
```

The icon uses the native Apple Color Emoji font for crisp rendering on a `#FFF8F0` cream background, 360×360 px (180pt @2x).
