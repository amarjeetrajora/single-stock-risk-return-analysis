# Reliance Industries: 15-Year Risk & Return Analysis

A historical, descriptive analysis of Reliance Industries' monthly return, risk, and market relationship (Jan 2011 – Dec 2025), benchmarked against the NIFTY 50.

## Problem Statement

Price appreciation alone doesn't describe an investment's characteristics. This project asks: **how has Reliance performed on a compounded basis, how much risk (volatility and drawdown) accompanied that performance, and how closely does it move with the broader Indian equity market?**

## Objectives

- Convert raw prices into monthly returns and compound them correctly into annual, cumulative, and CAGR figures
- Quantify risk two ways: volatility (dispersion) and maximum drawdown (worst realized loss)
- Estimate systematic risk (beta) and co-movement (correlation) against NIFTY 50, cross-validated two independent ways

## Key Findings

| Metric | Value |
|---|---|
| Observation period | Jan 2011 – Dec 2025 (monthly) |
| CAGR | **15.28%/year** |
| Cumulative return | 734.31% |
| Average monthly return | 1.47% |
| Monthly volatility | 7.56% |
| Annualized volatility | 26.19% |
| Maximum drawdown | **-33.34%** (trough: March 2020) |
| Best / worst month | Apr 2020 (+31.63%) / Mar 2020 (-16.17%) |
| Correlation with NIFTY 50 | 0.66 |
| Beta vs. NIFTY 50 | 1.07 |

*(CAGR of 15.28%/year is the figure comparable to a compounded annual rate — it differs from the naive ×12 annualized average monthly return of 16.30% because that calculation ignores compounding. See the notebook, Section 10.)*

![Cumulative Return](reports/figures/04_cumulative_return.png)
![Drawdown](reports/figures/05_drawdown.png)

## Dataset

- **Reliance Industries (RELIANCE.NS)** and **NIFTY 50 (^NSEI)**, monthly OHLCV, via `yfinance`, `auto_adjust=True`
- 180 monthly observations each, Jan 2011 – Dec 2025
- Verified: no missing values, no duplicate dates, no OHLC violations, dates fully aligned between the two series
- Prices are split/bonus-adjusted. The October 2024 1:1 Reliance bonus is checked in Section 6: the adjusted series shows no artificial ~50% price cliff.

## Methodology

Monthly closing prices → simple returns → compounded annual/cumulative returns and CAGR → volatility (sample std, annualized by √12) and maximum drawdown (peak-to-trough on the cumulative wealth index) → return distribution (skew/kurtosis) → merged with NIFTY 50 returns → covariance-based beta, cross-validated against `beta = correlation × (σ_stock / σ_market)`.

## Financial Concepts Used

Simple return, compounding, CAGR, standard deviation as volatility, annualization convention, maximum drawdown, skewness/kurtosis, covariance, correlation, beta (market model).

## Limitations

- No risk-free rate is assumed, so Sharpe/Sortino ratios are intentionally not computed
- Dividend adjustment cannot be verified from the downloaded data alone (split/bonus adjustment is verified; dividend treatment is not)
- Single stock vs. single benchmark — no sector or peer comparison, no diversification effects
- No transaction costs, taxes, or trading strategy are modeled
- This is a historical, descriptive study — not a forecast, and not investment advice

## Technologies Used

Python · pandas · NumPy · Matplotlib · yfinance · Jupyter Notebook

## Project Structure

```
risk-return-analysis/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   └── raw/
│       ├── Reliance_15_Year_Monthly.csv
│       └── Nifty_15_Year_Monthly.csv
│
├── notebooks/
│   └── Reliance_Risk_Return_Analysis.ipynb
│
└── figures/
    ├── 01_price_history.png
    ├── 02_monthly_returns.png
    ├── 03_annual_returns.png
    ├── 04_cumulative_return.png
    ├── 05_drawdown.png
    ├── 06_return_distribution.png
    └── 07_beta_scatter.png
 
    
```

## How to Run

```bash
git clone <repo-url>
cd single-stock-risk-return-analysis
pip install -r requirements.txt
jupyter notebook notebooks/Reliance_Risk_Return_Analysis.ipynb
```

The notebook loads the committed CSVs in `data/raw/` if present; otherwise it downloads fresh data via `yfinance` using the same acquisition parameters for both tickers, so results stay reproducible either way.

## Future Improvements

Sharpe/Sortino ratio with a stated risk-free-rate assumption, rolling volatility and rolling beta, sector/peer comparison, dividend-adjustment verification.

---
*Educational, historical quantitative analysis. Not investment advice.*
