# AI Bubble Monitor — Project Description
> "This Time is Different™"

## Overview

A real-time, single-file HTML dashboard that tracks leading indicators of an AI/tech market bubble and signals the probability of a significant correction (>20%). The dashboard is designed for financial analysts, investors, and researchers who want a quantitative, historically-grounded early-warning system for AI bubble risk.

**Live file:** `~/dev/ai-bubble-monitor/index.html`
**Target deploy:** GitHub Pages at `https://davidj026.github.io/ai-bubble-monitor/`
**Stack:** Pure HTML + CSS + Vanilla JS — zero dependencies, zero build step, single file.

---

## Architecture

### Single-File Constraint
The entire app lives in one `index.html` file. No frameworks, no bundlers, no external JS files. External fonts (Google Fonts) and no other external dependencies. Must remain deployable by dragging one file onto any static host.

### Design System
- **Theme:** Dark terminal / Bloomberg terminal aesthetic
- **Fonts:** `DM Serif Display` (headings/display), `Space Mono` (data/monospace), `IBM Plex Sans` (body)
- **Colors (CSS variables):**
  - `--bg: #080c10` — page background
  - `--bg2: #0d1117` — card background
  - `--bg3: #131a22` — hover/expanded state
  - `--green: #3fb950` — low risk / positive signal
  - `--yellow: #d29922` — caution / building risk
  - `--red: #f85149` — high risk / danger signal
  - `--blue: #58a6ff` — UI accents, links, timestamps
  - `--accent: #f0b429` — gold accent (italic heading)
- **Scanline overlay:** subtle CSS repeating-gradient on `body::before` for CRT effect

---

## Data Model

### Indicators Array (`INDICATORS`)
Each indicator object has these fields:

```js
{
  rank: Number,           // 1–10, reliability ranking
  name: String,           // Full indicator name
  short: String,          // Subtitle / short description
  status: 'green' | 'yellow' | 'red',
  current: String,        // Current value (verbose)
  currentShort: String,   // Current value (compact, shown in table)
  threshold: String,      // Red flag threshold description
  barPct: Number,         // 0–100, risk percentage for progress bar
  historical: String,     // Historical precedent paragraph
  currentDetail: String,  // Current May 2026 status paragraph
  redFlag: String,        // Specific falsifiable trigger condition
  source: String,         // URL to primary data source
  sourceLabel: String,    // Human-readable source label
  frequency: String,      // How often to check (DAILY / WEEKLY / etc.)
}
```

### Checklist Items (`CHECKLIST_ITEMS`)
```js
{
  title: String,   // Action title
  desc: String,    // Description of what to check
  freq: String,    // Frequency label
  key: String,     // Unique key for state tracking
}
```

### Log Entries (`INIT_LOGS`)
```js
{
  msg: String,                    // Log message
  tag: 'red' | 'yellow' | 'green', // Severity
  time: String,                   // Display timestamp (e.g. "May 01")
}
```

---

## UI Components

### 1. Title Banner
Large serif display: **"This Time is Different™"** — centered, full-width, above the header.

### 2. Header
- Left: Brand dot (pulsing red) + "Live Signal Monitor · AI/Tech Bubble Risk" label + `<h1>` with "The *AI Bubble* Monitor"
- Right: Live clock (ticking every second via `setInterval`) + overall risk score badge

### 3. Risk Gauge (SVG)
- Half-circle arc colored green → yellow → red via `linearGradient`
- Animated needle pointing to current risk score position
- Needle uses CSS `transform: rotate(Xdeg)` with cubic-bezier transition
- Score range: 0–10, mapped to -90deg (safe) → +90deg (crash)
- Current score: **6.4 / 10** → needle at ~+22deg

### 4. Market Snapshot Strip
8 macro cards in a CSS grid (`repeat(auto-fit, minmax(150px, 1fr))`).
Each card has a colored top border (2px), label, large value, sub-label, and change indicator.

### 5. Signal Indicators (Expandable Rows)
10 rows, each showing: rank, name/subtitle, risk bar, current value, status badge.
**Click to expand** reveals a 3-column detail panel: Historical Precedent | Current Status | Red Flag Threshold + source link + check frequency.

### 6. Activity Log
Scrollable feed with columns: timestamp | message | severity tag.
New entries prepended via `addLog(msg, tag)`. Initial entries hardcoded in `INIT_LOGS`.

### 7. Monitoring Checklist
12 clickable cards in a 2-column grid. Click toggles `checked` state (green border + checkmark). Checking an item logs it to the activity feed.

### 8. Scenario Analysis
3 cards (Bull / Base / Bear) with probability percentages, descriptions, triggers, and lead times.

### 9. Footer
Disclaimer text + "Refresh Signals" button (simulates a spinner + log entries).

---

## Key Functions

| Function | Description |
|---|---|
| `renderIndicators()` | Builds all 10 indicator rows from `INDICATORS` array |
| `toggleExpand(i)` | Toggles expanded detail panel on indicator row `i` |
| `renderChecklist()` | Builds all 12 checklist cards from `CHECKLIST_ITEMS` |
| `toggleCheck(key, el)` | Toggles checked state + logs completion |
| `updateTime()` | Updates `#live-time` every second |
| `renderInitLogs()` | Populates activity log with `INIT_LOGS` entries |
| `addLog(msg, tag)` | Prepends new entry to activity log with flash animation |
| `setNeedle(score)` | Rotates gauge needle to position for score 0–10 |
| `simulateRefresh()` | Animates refresh button + adds log entries |

---

## Current Data (May 2026 Snapshot)

| Indicator | Value | Status |
|---|---|---|
| S&P 500 | 7,230 | Yellow |
| Shiller CAPE | ~39× | Red |
| Forward P/E | 20.9× | Yellow |
| VIX | 16.99 | Yellow |
| WTI Crude | ~$107 (+70% YTD) | Red |
| Fed Funds Rate | 3.75% (on hold) | Yellow |
| Hyperscaler Capex 2026 | $725B | Red |
| S&P EPS Growth '26 | +21.3% | Green |
| Overall Risk Score | 6.4 / 10 | Elevated |

---

## Planned Enhancements (Backlog)

### High Priority
- [ ] **Live data API integration** — connect macro cards to free APIs:
  - VIX: CBOE data feed or Yahoo Finance
  - S&P 500 price: Yahoo Finance or Alpha Vantage
  - WTI Crude: EIA API (free, no key required)
  - 10yr Treasury: FRED API (`api.stlouisfed.org`)
- [ ] **Editable indicator values** — click any macro card value to edit inline, auto-saves to `localStorage`
- [ ] **Alert system** — user sets threshold for each indicator; browser notification fires when crossed
- [ ] **Weekly snapshot persistence** — save current indicator states to `localStorage` keyed by ISO week, display historical trend

### Medium Priority
- [ ] **Shareable URL state** — encode current indicator statuses in URL hash for sharing specific views
- [ ] **Export to PDF** — one-click print-optimized layout using `window.print()` with `@media print` styles
- [ ] **Historical chart** — sparkline chart (using SVG paths) showing CAPE, VIX, or S&P over time
- [ ] **Mobile layout** — already partially responsive; improve stacking on narrow screens

### Low Priority
- [ ] **Dark/light theme toggle**
- [ ] **Internationalization** — support non-USD currencies for oil/capex figures
- [ ] **Embed widget** — `<iframe>`-embeddable version with minimal chrome

---

## Development Guidelines for AI Agents

### Rules
1. **Never break the single-file constraint.** All CSS and JS stays inside `index.html`. No separate files.
2. **Preserve the design system.** Use only the CSS variables defined in `:root`. Do not add new colors inline — add new variables to `:root` if needed.
3. **Keep the data model typed.** When modifying `INDICATORS` or `CHECKLIST_ITEMS`, maintain all required fields for every entry.
4. **Data accuracy matters.** All financial figures, historical precedents, and thresholds are research-backed (sourced from FactSet, FRED, CBOE, EIA). Do not change data values without explicit instruction and sourcing.
5. **Functional animations only.** The gauge needle, pulse dot, and log flash animations serve a UX purpose. Don't add decorative animations that distract.
6. **Accessibility.** Keep `aria-label` on SVG elements. Ensure interactive elements are keyboard-reachable.

### When Adding a New Indicator
1. Add a new object to `INDICATORS` with all 11 fields populated
2. Assign the next sequential `rank`
3. Set `barPct` based on honest risk assessment (0 = no risk, 100 = maximum danger)
4. Include a real `source` URL and `sourceLabel`
5. `frequency` should be one of: `DAILY`, `WEEKLY`, `MONTHLY`, `QUARTERLY`, or `MEETING-BY-MEETING`

### When Connecting a Live API
- Use `fetch()` inside an async init function called on page load
- Gracefully degrade: if fetch fails, show the hardcoded fallback values
- Display a subtle "live" indicator (e.g., green dot) on cards that are receiving real data
- Never expose API keys in client-side code — use only keyless or CORS-friendly public endpoints

### File Locations
- **Source file:** `/root/dev/ai-bubble-monitor/index.html`
- **Backup:** `/root/home/claude/index.html`
- **GitHub repo:** `https://github.com/DavidJ026/ai-bubble-monitor`
- **Target Pages URL:** `https://davidj026.github.io/ai-bubble-monitor/`

---

## How to Deploy

```bash
cd ~/dev/ai-bubble-monitor
git add index.html
git commit -m "Update: <describe change>"
git push origin main
# GitHub Pages auto-deploys from main branch root
# Live in ~60 seconds at https://davidj026.github.io/ai-bubble-monitor/
```

*First-time setup requires a GitHub Personal Access Token with `repo` scope.*
*Create at: github.com/settings/tokens → Generate new token (classic) → check `repo`.*
