# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file HTML dashboard tracking leading indicators of an AI/tech market bubble. Zero dependencies, zero build step — the entire app lives in one HTML file (`ai-bubble-monitor.html`). Deploy target: GitHub Pages at `https://davidj026.github.io/ai-bubble-monitor/`.

## No Build Commands

There is no build, lint, or test toolchain. Open `index.html` directly in a browser to develop. Deployment is:

```bash
git add index.html
git commit -m "Update: <describe change>"
git push origin main
# GitHub Pages deploys in ~60 seconds
```

## Architecture

The file is structured as: `<style>` block → inline `<script>` block → `<body>` markup. All CSS and JS must remain inside `index.html` — no external files, ever.

### Data Model

Three core JS arrays drive all content:

**`INDICATORS`** — 10 risk indicator objects, each with: `rank`, `name`, `short`, `status` (`'green'|'yellow'|'red'`), `current`, `currentShort`, `threshold`, `barPct` (0–100), `historical`, `currentDetail`, `redFlag`, `source` (URL), `sourceLabel`, `frequency`.

**`CHECKLIST_ITEMS`** — 12 monitoring task objects: `title`, `desc`, `freq`, `key`.

**`INIT_LOGS`** — Initial activity log entries: `msg`, `tag`, `time`.

### Key Functions

| Function | Purpose |
|---|---|
| `renderIndicators()` | Builds all 10 expandable indicator rows |
| `toggleExpand(i)` | Toggles detail panel on row `i` |
| `renderChecklist()` | Builds checklist cards |
| `toggleCheck(key, el)` | Toggles checked state + logs it |
| `addLog(msg, tag)` | Prepends entry to activity log |
| `setNeedle(score)` | Rotates SVG gauge needle (score 0–10 → -90° to +90°) |
| `simulateRefresh()` | Animates refresh button + adds log entries |

### Design System

CSS variables in `:root` — always use these, never add inline colors:
- `--bg / --bg2 / --bg3` — background layers
- `--green / --yellow / --red` — risk signal colors (with `-dim` variants for backgrounds)
- `--blue` — UI accents; `--accent` — gold italic heading
- `--mono / --serif / --sans` — Space Mono / DM Serif Display / IBM Plex Sans

## Rules

1. **Single-file constraint is absolute.** No separate JS/CSS files.
2. **Use only `:root` CSS variables.** Add new variables to `:root` if needed; never inline new colors.
3. **Data accuracy matters.** Financial figures are sourced from FactSet, FRED, CBOE, EIA. Don't change values without explicit instruction and sourcing.
4. **When adding a new indicator:** populate all fields, set `barPct` honestly, include a real `source` URL, and use one of: `DAILY / WEEKLY / MONTHLY / QUARTERLY / MEETING-BY-MEETING` for `frequency`.
5. **When connecting a live API:** use `fetch()` in an async init, degrade gracefully to hardcoded fallbacks, never expose API keys in client-side code (use only keyless/CORS-friendly public endpoints).
6. **Keep `aria-label` on SVG elements** and ensure interactive elements are keyboard-reachable.
