Before update index.html
- Create a new file by copying index.html to index[Last updated].html. The last updated date is within index.html.

Steps to update index.html

STEP 1 — RESEARCH: Web search these data points for the current date:
- S&P 500 and Nasdaq closing level + % yr/yr change
- VIX current level
- WTI crude oil price
- Fed funds rate (any changes or new FOMC statements)
- Shiller CAPE ratio (multpl.com)
- Any Mag 7 earnings reports or guidance changes this week
- Nvidia: stock price, any revenue/margin news, China export status
- Hyperscaler capex: any raised/cut guidance from MSFT/AMZN/GOOGL/META
- Key AI bubble news (SaaS selloffs, new warnings, VC funding, ROI evidence)

STEP 2 — SCORE: Reassess each of the 10 INDICATORS in the JS array:
- Update status: 'green' | 'yellow' | 'red'
- Update currentShort and current values
- Update barPct (0–100 risk %)
- Update currentDetail paragraph with new data + date stamp

STEP 3 — UPDATE these HTML/JS fields:
- document.getElementById('live-time').textContent → today's date
- #risk-score .value → recalculate overall score (avg of indicator barPct / 10)
- setNeedle(X) → match new score
- .gauge-sublabel text → current month/date
- .gauge-label text → Low / Moderate / Elevated / High / Critical
- 8 macro-card values: S&P 500, CAPE, Forward P/E, VIX, WTI, Fed Rate, Capex, EPS Growth
- Each macro-card color class (green/yellow/red) based on new values
- INIT_LOGS array → prepend 3–5 new log entries with today's date, tag, message
- 3 scenario probabilities → adjust based on new macro picture
- scenario-desc text → update with latest triggers and context
- footer note → update date and key changes summary

STEP 4 — RULES:
- Never change CSS, layout, or structure — data only
- Keep all 10 indicators in INDICATORS array; only update values
- Tag colors: green = improving/safe, yellow = caution, red = danger/worsening
- Risk score 0–4 = Low, 4–6 = Moderate, 6–7.5 = Elevated, 7.5–9 = High, 9+ = Critical
- Scenario probabilities must sum to 100%
- Always cite the data source and date in currentDetail text
- Output the full updated index.html file