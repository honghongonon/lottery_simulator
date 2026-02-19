# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Korean Lotto 6/45 simulator — a single-page static web app (`index.html`) that lets users experience lottery odds through rapid simulation. No build step, no bundler, no package manager. Everything (HTML, CSS, JS) lives in one file.

## Development

Open `index.html` directly in a browser. There is no build, lint, or test process.

## Architecture

### Single-file structure
All code is in `index.html`: CSS in `<style>`, HTML in `<body>`, JavaScript in two `<script>` blocks at the end of `<body>`.

### Key state (global JS variables)
- `myNumbers` — user's 6 picked numbers
- `running` / `animFrameId` — simulation loop control
- `totalDraws`, `totalWinnings`, `winCounts` — cumulative stats
- `currentSpeed` — draws per second (100–1000), controlled by `SPEED_MAP`
- `hofData` — Hall of Fame leaderboard data (`biggestLoss` / `cheapWin` arrays)

### Simulation loop (`runLoop`)
Uses `requestAnimationFrame` with batch processing (`batchSize` scaled to `currentSpeed`). UI updates are throttled to every 100ms via `UI_INTERVAL`. Win effects (fireworks, mega-win banners) are queued in `pendingWins` and processed during UI update ticks.

### Firebase integration
Uses Firebase Realtime Database (compat SDK v10.12.2) loaded via CDN for the shared Hall of Fame leaderboard. Database reference: `db.ref('hof-leaderboard')`. Data syncs in real-time via `hofRef.on('value', ...)`.

### Prize constants
`PRIZE = { 1: 2,000,000,000, 2: 30,000,000, 3: 2,000,000, 4: 50,000, 5: 5,000 }`, ticket price 1,000 KRW.

### Lotto rules
Numbers 1–45, pick 6 main + 1 bonus. Ball color classes (`range1`–`range5`) are assigned by decade (1–10, 11–20, etc.).

### UI sections
1. **Live ticker** — scrolling marquee of HOF entries
2. **Hall of Fame** — two tabs (biggest loss / cheapest 1st-place win), nickname registration, Firebase-backed
3. **Simulator** — number generation, draw roulette display, start/stop/reset, speed slider
4. **Stats** — draws, investment, winnings, profit, ROI, per-rank prize table, break-even analysis with progress bar, year/timeline display
5. **Share** — Kakao, Twitter, clipboard copy, native share API
6. **SEO/About sections** — probability info, FAQ

### External services
- Firebase Realtime Database (config in `firebaseConfig`)
- Google AdSense (`ca-pub-7327747719856671`)
- Google Analytics (`G-TF0DV7T73T`)
- Naver site verification

## Language

All UI text is in Korean. Maintain Korean for user-facing strings.
