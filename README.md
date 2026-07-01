# Market Risk Monitor
An automated market risk reporting system that produces a weekly, self-contained HTML stress report for a multi-desk trading book (Rates, FX, Credit). The report is regenerated every Monday at 07:00 UTC.

**[View latest report →](https://razzasingh.github.io/MarketRiskMonitor)**

---

## What it does

The notebook pulls live market data via Yahoo Finance and runs a full risk pipeline end-to-end:

- **Trading book construction** — multi-desk portfolio (Rates, FX, Credit) with long/short positions, governance controls (gross exposure cap, desk concentration limits)
- **Historical VaR & ES** — 99% VaR and 97.5% Expected Shortfall, with desk-level decomposition; diversification benefit (ES) reported as a key summary metric
- **Stressed VaR & ES** — calibrated to a fixed historical stress window; SVaR/VaR ratio tracked as a time-series trend chart — a model health indicator showing how current VaR drifts relative to the fixed stress benchmark
- **Liquidity-adjusted VaR** — FRTB-style liquidity horizon scaling per instrument (10–20 day horizons), producing a LA-VaR alongside exposure-weighted average liquidity horizon
- **Capital proxy (FRTB-style)** — model capital estimated as 1.5× max(ES, Stressed ES), plus a combined RNIM add-on (liquidity, concentration, JTD, correlation) shown as a single capital overlay figure
- **Scenario stress testing** — explicit return-space shocks across rate, credit, FX, and macro scenarios, with desk-, factor-, and instrument-level attribution and concentration metrics (top-1/3/5 share of P&L) per scenario
- **Reverse stress testing** — computes the minimum shock multiplier required to breach the capital loss threshold for each scenario; also reused as a "stress buffer" KRI
- **Backtesting & validation** — rolling 250-day exception count, Kupiec p-value, VaR limit utilisation, all RAG-rated (GREEN/AMBER/RED)
- **Data quality checks** — missing data, stale prices, extreme return detection per ticker
- **RNIM register** — Risks Not In Model governance log with live signals where modelled (liquidity gap, correlation breakdown, JTD, proxy/basis risk)
- **Non-financial risk module** — RCSA table for risks specific to the model and its operation (data feed failure, stale price data, model/methodology error, broken correlation assumptions, manual override risk, calibration drift), plus a KRI dashboard with RAG status reframed entirely from existing model outputs (VaR utilisation, backtesting exceptions, data quality failures, RNIM add-ons flagged, stress buffer)

The output is a single self-contained HTML file with embedded charts and styled tables. No external dependencies to view it.

---

## Tech stack

| Area | Tools |
|---|---|
| Data | `yfinance`, `pandas` |
| Risk & numerics | `numpy`, `math` (standard library, Kupiec test) |
| Charting | `matplotlib` |
| Report | HTML/CSS (self-contained) |
| Automation | GitHub Actions (CI/CD) |
| Hosting | GitHub Pages |

---

## How it runs

The report is regenerated automatically every Monday at 07:00 UTC via a GitHub Actions workflow. On each run it:

1. Pulls the latest market prices from Yahoo Finance
2. Executes the notebook end-to-end
3. Publishes the HTML output to GitHub Pages

---

## Project structure

```
├── Market Risk Monitor V4.ipynb   # Main notebook
├── .github/
│   └── workflows/
│       └── run_notebook.yml       # CI/CD pipeline
└── outputs/                       # Generated HTML reports (gitignored locally)
```

---

## Disclaimer

This is a portfolio/toy project. The trading book, positions, and risk figures are illustrative and do not represent real trades or financial advice.
