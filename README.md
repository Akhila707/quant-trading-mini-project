# Risk-Aware Trading Insights from Volatility and Volume Behavior

**Quantitative Research — Technology Equities Internship Assignment**  
**Author:** Akhila  
**Period:** January 2023 – January 2026  
**Sector:** Technology Equities

---

## Overview

This project investigates the price, volatility, and trading volume behaviour of five large-cap US technology stocks over a three-year window. The goal was to identify simple, observable market patterns and test whether they could form the basis of a short-term rule-based trading strategy.

The work follows a standard quantitative research pipeline: data collection → feature engineering → exploratory analysis → pattern identification → strategy design → backtesting → risk assessment.

No machine learning or advanced statistical methods are used. Everything is built from scratch using standard Python data libraries.

---

## Universe

| Ticker | Company | Role in Study |
|--------|---------|---------------|
| AAPL | Apple Inc. | Stable large-cap baseline |
| NVDA | NVIDIA Corporation | Momentum stock / negative control |
| MSFT | Microsoft Corporation | Moderate-volatility benchmark |
| TSLA | Tesla Inc. | High-volatility mean-reversion candidate |
| AMZN | Amazon.com Inc. | Mixed momentum and reversion behaviour |

---

## Project Structure

```
.
├── Quant_Internship_Akhila_PV.ipynb   # Main analysis notebook
├── Quant_Internship_Report.pdf        # Academic-style PDF report
├── Quant_Internship_Report.tex        # LaTeX source for the report
└── README.md                          # This file
```

---

## Notebook Walkthrough

The notebook is organised into nine sections:

1. **Data Collection** — Downloads daily OHLCV data via `yfinance` for all five tickers.
2. **Data Cleaning & Feature Engineering** — Forward-fills missing values, applies a 40-day warm-up, and computes log returns, 20-day rolling volatility, volume averages, and drawdown series.
3. **Exploratory Data Analysis** — Four charts: normalised price performance, rolling volatility, drawdown from peak, and normalised volume trends.
4. **Pattern Identification** — Three patterns tested with statistical checks:
   - NVDA momentum persistence (Welch t-test)
   - TSLA mean reversion after large drops (binomial test)
   - Volume–volatility co-movement (Pearson correlation)
5. **Trading Strategy Definition** — Dual-condition dip-buying signal: daily return below −5% AND current volatility above its 20-day average.
6. **Backtesting** — Custom event-driven engine with t+1 entry (no look-ahead bias), daily mark-to-market, and no overlapping trades. Nine performance metrics computed per ticker.
7. **Risk Analysis** — Win/loss ratio table, transaction cost sensitivity sweep (0–50 bps), and volatility regime chart at trade entries.
8. **Conclusion** — Results summary with actual numbers; NVDA negative control explicitly resolved.
9. **AI Usage Disclosure** — Transparent breakdown of what was AI-assisted and what was done independently.

---

## Strategy Summary

**Type:** Short-term dip-buying (mean reversion)

**Entry signal — both conditions must be true on day *t*:**
- Daily log return < −5%
- 20-day rolling volatility > its own 20-day average

**Execution:**
- Enter at the close of day *t+1* (next-day fill — no look-ahead)
- Exit at the close of day *t+1+3* (3-day hold)
- Skip new signals while a trade is open (no overlapping positions)

---

## Key Results

| Ticker | Trades | Win Rate | Strategy Return | Buy & Hold |
|--------|--------|----------|-----------------|------------|
| AMZN | 6 | 66.7% | +24.1% | +150.5% |
| TSLA | 28 | 50.0% | +11.5% | +135.6% |
| AAPL | 1 | — | — | ~+70% |
| MSFT | 2 | — | — | ~+60% |
| NVDA | 14 | 35.7% | −35.6% | +700.8% |

NVDA's poor strategy result was expected — it served as a deliberate negative control to confirm that mean-reversion logic fails on strongly trending momentum stocks.

---

## Installation

```bash
pip install yfinance pandas numpy matplotlib scipy
```

Then open the notebook:

```bash
jupyter notebook Quant_Internship_Akhila_PV.ipynb
```

Python 3.8 or higher is recommended.

---

## Dependencies

| Library | Purpose |
|---------|---------|
| `yfinance` | Download historical market data |
| `pandas` | Data manipulation and time series handling |
| `numpy` | Numerical calculations and array operations |
| `matplotlib` | All charts and visualisations |
| `scipy` | Statistical tests (Welch t-test, binomial test) |

---

## Limitations

- **Survivorship bias** — Only large-cap stocks that performed well over the period were selected. A random universe would likely produce weaker results.
- **Fully in-sample** — All parameters were chosen after observing the data. No out-of-sample or walk-forward validation was performed.
- **Small trade count** — AAPL (1 trade) and MSFT (2 trades) do not produce statistically meaningful results.
- **No execution costs** — Real trading would include slippage, bid-ask spread, and commissions that would erode returns further.
- **No risk management** — No stop-losses, position limits, or portfolio-level controls are applied.

---

## AI Usage

Claude (Anthropic) was used as a coding assistant to help draft and debug portions of the backtest engine and chart formatting code. Pattern selection, strategy design, all analytical conclusions, and editorial decisions were made independently. Every AI-suggested code block was read, tested, and validated before inclusion.

---

## Disclaimer

This project is for educational purposes only. Nothing in this notebook or report constitutes investment advice. Past backtest performance does not guarantee future results.
