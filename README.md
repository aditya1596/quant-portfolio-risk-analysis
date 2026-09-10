# Magnificent 7 Portfolio Optimization & Risk Forecasting

An end-to-end quantitative finance project analyzing the **Magnificent 7 stocks (AAPL, GOOG, META, AMZN, MSFT, NVDA, TSLA)** using portfolio theory, statistical analysis, and Monte Carlo simulation to evaluate performance, optimize allocation, and forecast downside risk.

---

## Highlights

* Achieved **~252.6% cumulative portfolio return** (equal-weight) over 5 years vs **~85.0% for the S&P 500** and **~104.7% for QQQ**
* Fixed look-ahead bias with a proper **train/test split**: portfolio weights are estimated on 2021–2023 data and all evaluation (Monte Carlo, VaR, backtest) runs on a held-out 2024–2025 window
* Constructed the **Efficient Frontier** using **20,000 simulated portfolios** under a 50% single-stock concentration cap
* Performed **Monte Carlo simulations (10,000 paths)** for portfolio- and stock-level risk forecasting using Cholesky-decomposed correlated shocks
* Reduced simulated portfolio downside risk from **-29.02% (Equal-Weight) to -24.71% (50%-capped Optimal) 95% VaR**
* Added **CAPM/alpha analysis** with a dynamically sourced risk-free rate (10-Year US Treasury yield, `^TNX`)
* Added **Maximum Drawdown analysis** and a **transaction-cost-aware** (5 bps/trade) rebalancing backtest

---

## Project Objective

Most stock analyses focus only on returns. This project evaluates investments through a broader quantitative framework that incorporates:

* Return
* Risk
* Diversification
* Portfolio Optimization
* Downside Risk
* Future Uncertainty

The goal is to determine whether the Magnificent 7 can deliver superior risk-adjusted performance and how capital should be allocated across them.

---

## Dataset

| Parameter    | Details                                  |
| ------------ | ----------------------------------------- |
| Source       | Yahoo Finance (`yfinance`)               |
| Stocks       | AAPL, GOOG, META, AMZN, MSFT, NVDA, TSLA |
| Benchmarks   | S&P 500 (`^GSPC`), QQQ ETF               |
| Risk-Free Rate | 10-Year US Treasury Yield (`^TNX`), sourced dynamically |
| Time Horizon | 5 Years (2021–2025)                      |
| Frequency    | Daily Adjusted Closing Prices            |
| Estimation Window | 2021–2023 (used to fit portfolio weights) |
| Evaluation Window  | 2024–2025 (held out for Monte Carlo, VaR, backtest) |

The selected period captures multiple market environments, including the COVID recovery, the 2022 rate-hike-driven correction, and the AI-led technology rally.

---

## Analysis Pipeline

### 1. Exploratory Data Analysis
* Historical price trends
* Normalized growth comparison (Base = 100)
* SMA vs EMA trend analysis (10/25/50-day, Google deep dive)
* Daily return distributions

### 2. Statistical Analysis
* Daily return calculation
* D'Agostino-Pearson normality testing (K² test) with Q-Q plots
* Volatility analysis
* Correlation heatmap

### 3. Risk & Performance Metrics
* Annualized Return
* Annualized Volatility
* Beta
* Sharpe Ratio
* CAPM Expected Return
* Jensen-style Alpha

### 4. Benchmark Comparison
Performance comparison against:
* S&P 500
* QQQ ETF

Additional evaluation:
* Cumulative excess return vs. each benchmark
* Maximum Drawdown

### 5. Portfolio Optimization
Using Modern Portfolio Theory (Markowitz Framework), fit on the 2021–2023 estimation window only:
* Simulated **20,000 random portfolios** via constrained Dirichlet sampling
* Calculated Return, Volatility, and Sharpe Ratio for each
* Constructed the Efficient Frontier
* Reported both the **unconstrained** maximum-Sharpe portfolio (diagnostic) and a **50% single-stock-cap** maximum-Sharpe portfolio (final allocation) to make the concentration trade-off explicit

### 6. Monte Carlo Risk Forecasting
* Geometric Brownian Motion (GBM), driven by historical daily log-return mean/volatility
* Cholesky decomposition of the log-return covariance matrix for correlated multi-asset simulation
* **10,000 future paths** over a 1-year (252-trading-day) horizon, simulated forward from the 2024 evaluation start date
* Individual NVDA price-path simulation as a single-stock deep dive

### 7. Value at Risk (VaR)
* Portfolio 95% VaR (5th percentile of simulated 1-year return distribution)
* Individual NVDA 99% VaR
* Downside risk comparison between Equal-Weight and 50%-capped Optimized portfolios

### 8. Portfolio Backtesting
Comparison over the 2024–2025 evaluation window, including a 5-bps-per-trade transaction cost assumption:
* Buy & Hold
* Monthly Rebalancing

to evaluate diversification benefits, turnover cost, and long-term performance.

---

## Key Results

| Metric                                   | Result     |
| ----------------------------------------- | ---------- |
| M7 Equal-Weight Cumulative Return          | +252.6%    |
| S&P 500 Cumulative Return                  | +85.0%     |
| QQQ Cumulative Return                      | +104.7%    |
| M7 Maximum Drawdown                        | -49.4%     |
| S&P 500 Maximum Drawdown                   | -25.4%     |
| QQQ Maximum Drawdown                       | -35.1%     |
| Portfolios Simulated                       | 20,000     |
| Monte Carlo Paths                          | 10,000     |
| Unconstrained Max-Sharpe                   | 1.025      |
| 50%-Capped Max-Sharpe (final portfolio)    | 0.980      |
| 50%-Capped Annualized Return               | 40.8%      |
| 50%-Capped Annualized Volatility           | 37.3%      |
| Equal-Weight Portfolio 95% VaR             | -29.0%     |
| 50%-Capped Optimal Portfolio 95% VaR       | -24.7%     |
| NVDA 99% VaR (1-yr simulated)              | -49.5%     |
| Monthly Rebalancing Return (net of costs)  | +106.9%    |
| Buy & Hold Return                          | +107.2%    |

Final 50%-capped optimal allocation: **50% MSFT / 50% NVDA** (all other names at 0%, driven by the concentration cap binding on the two highest-Sharpe names in the estimation window).

---

## Major Findings

* NVIDIA was the strongest performer across most return-based metrics during the study period (annualized return ~67%, Sharpe ~1.20, CAPM alpha ~+42%).
* All seven stocks reject the normality assumption (D'Agostino-Pearson, p < 0.05), confirming fat-tailed return distributions — a key motivation for treating VaR as a distributional quantile rather than a hard bound.
* The Magnificent 7 significantly outperformed both the S&P 500 and QQQ benchmarks, but with roughly double the maximum drawdown of the S&P 500.
* Enforcing a 50% single-stock cap reduces the Sharpe Ratio only modestly (1.025 → 0.980) while controlling concentration risk versus the unconstrained optimizer, which would otherwise concentrate almost entirely in the top 1–2 names.
* Fixing the look-ahead bias (fitting weights on 2021–2023 and evaluating strictly on 2024–2025) means the reported optimized-portfolio numbers reflect genuinely out-of-sample performance, not in-sample overfitting.
* After accounting for a 5-bps transaction cost per trade, monthly rebalancing and buy-and-hold produced nearly identical net returns (106.9% vs 107.2%) over the evaluation window — rebalancing's diversification benefit was largely offset by turnover cost and by trimming the strongest performers during a persistent rally.
* Positive correlations across all seven stocks (highest: MSFT–AMZN at 0.655) indicate limited diversification within the group alone.

---

## Tech Stack

* Python
* Pandas
* NumPy
* SciPy
* Matplotlib
* Seaborn
* yFinance
* Jupyter Notebook

---

## Contact

**Aditya Singh Shekhawat | IIT Indore — (Final Year)**

[![GitHub](https://img.shields.io/badge/GitHub-aditya1596-black)](https://github.com/aditya1596)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Aditya-blue)](https://www.linkedin.com/in/aditya-singh-shekhawat-934930288/)
