# Market Risk Monitor

An automated market risk reporting system that produces a daily, self-contained HTML stress report for a multi-desk trading book (Rates, FX, Credit). The report is generated every weekday at 08:00 UTC

**[View latest report →](https://razzasingh.github.io/MarketRiskMonitor)** 

---

## What it does

The notebook pulls live market data via Yahoo Finance and runs a full risk pipeline end-to-end:

- **Trading book construction** — multi-desk portfolio (Rates, FX, Credit) with long/short positions, governance controls (gross exposure cap, desk concentration limits)
- **Historical VaR & ES** — 99% VaR and 97.5% Expected Shortfall, with desk-level decomposition and diversification benefit tracking
- **Stressed VaR & ES** — calibrated to a fixed historical stress window; SVaR/VaR ratio computed as a model health indicator
- **Capital proxy (FRTB-style)** — model capital estimated as 1.5× max(ES, Stressed ES), plus RNIM add-ons
- **Scenario stress testing** — explicit return-space shocks across scenarios, with concentration metrics (HHI, top-share) per scenario
- **Reverse stress testing** — computes the minimum shock multiplier required to breach a loss threshold for each scenario
- **Portfolio change attribution** — T0 vs T1 comparison showing desk- and risk-factor-level drivers of stress change
- **Backtesting & validation** — rolling 250-day exception count, Kupiec p-value, VaR limit utilisation, tail event flagging
- **Data quality checks** — missing data, stale prices, extreme return detection per ticker
- **RNIM register** — Risks Not In Model governance log (liquidity, correlation, JTD, proxy/basis risk)

The output is a single self-contained HTML file with embedded charts and styled tables — no external dependencies to view it.

---

## Tech stack

| Area | Tools |
|---|---|
| Data | `yfinance`, `pandas` |
| Risk & numerics | `numpy`, `scipy` (implied) |
| Charting | `matplotlib` |
| Report | HTML/CSS (self-contained) |
| Automation | GitHub Actions (CI/CD) |
| Hosting | GitHub Pages |

---

## How it runs

The report is regenerated automatically every weekday at 08:00 UTC via a GitHub Actions workflow. On each run it:

1. Pulls the latest market prices from Yahoo Finance
2. Executes the notebook end-to-end
3. Publishes the HTML output to GitHub Pages

---

## Project structure

```
├── Market Risk Monitor.ipynb   # Main notebook
├── .github/
│   └── workflows/
│       └── run_notebook.yml    # CI/CD pipeline
└── outputs/                    # Generated HTML reports (gitignored locally)
```

---

## Disclaimer

This is a portfolio/toy project. The trading book, positions, and risk figures are illustrative and do not represent real trades or financial advice.
