# Magnificent 7 Portfolio Optimization & Risk Forecasting

An end-to-end quantitative finance project analyzing the **Magnificent 7 stocks (AAPL, GOOG, META, AMZN, MSFT, NVDA, TSLA)** using descriptive risk analysis, CAPM, Modern Portfolio Theory, out-of-sample validation, and Monte Carlo simulation.

---

## Highlights

* Out-of-sample **Sharpe ratio of 0.89** (cumulative return 379.7%) on a held-out 2021–2025 window, vs **0.52** for the S&P 500 and **0.53** for QQQ
* Portfolio weights are estimated **only on 2016–2020 data**; the 2021–2025 evaluation window is never used to fit them
* Efficient Frontier built from **20,000** randomly sampled long-only portfolios (Dirichlet sampling)
* **10,000 correlated Monte Carlo paths** (GBM with Cholesky decomposition) for portfolio risk, giving a 95% VaR of **−7.29%** for equal-weight and **−5.57%** for the max-Sharpe portfolio
* Single-stock **NVDA Monte Carlo** with a 99% simulated VaR of **−46.05%**
* Average pairwise correlation of **0.541** across the seven stocks; normality rejected for all seven (D'Agostino–Pearson, p < 0.0001)

---

## Project Objective

Most stock analyses focus on returns alone. This project evaluates the Magnificent 7 through a broader framework covering return, risk, diversification, portfolio optimization, downside risk, and out-of-sample validation. The central question is whether optimized weights estimated on historical data still deliver good risk-adjusted performance on data they were not fitted to.

---

## Dataset

| Parameter | Details |
|---|---|
| Source | Yahoo Finance (`yfinance`), daily adjusted closing prices |
| Stocks | AAPL, GOOG, META, AMZN, MSFT, NVDA, TSLA |
| Benchmarks | S&P 500 (`^GSPC`), QQQ ETF |
| Full history | 2016–2025 (2,514 trading days) |
| Estimation window | 2016–2020 (fits optimizer weights and Monte Carlo parameters) |
| Evaluation window | 2021–2025 (held out; out-of-sample tests) |
| Risk-free rate | 5.0% annual (fixed assumption) |
| Annualization | 252 trading days |

Descriptive statistics (return, volatility, Beta, Sharpe, correlation, drawdown, CAPM) use the full 2016–2025 history. The train/test split applies to every model that is fitted and every decision that is made.

---

## Analysis Pipeline

1. **Exploratory data analysis:** price trends, normalized growth (base 100), SMA/EMA trend analysis (10/25/50-day, with a Google deep dive)
2. **Statistical tests:** daily return distributions, D'Agostino–Pearson normality tests, Q-Q plots, correlation heatmap
3. **Risk and return:** annualized return and volatility, Beta, Sharpe ratio
4. **Benchmarking:** equal-weight M7 vs S&P 500 and QQQ, cumulative excess return, maximum drawdown
5. **CAPM:** expected return and Jensen-style alpha per stock
6. **Portfolio optimization:** Modern Portfolio Theory fitted on 2016–2020 (20,000 Dirichlet portfolios, SLSQP maximum-Sharpe and minimum-variance solutions)
7. **Out-of-sample validation:** fitted weights applied to 2021–2025 and compared against equal-weight, S&P 500 and QQQ
8. **Monte Carlo risk forecasting:** NVDA single-stock GBM (10,000 paths) and correlated seven-stock simulation (10,000 paths, 2016–2020 parameters, starting from January 2021 prices)
9. **Value at Risk:** 95% VaR and CVaR for equal-weight and max-Sharpe portfolios; 99% VaR for NVDA
10. **Rebalancing backtest:** monthly rebalancing vs constant-weight daily rebalancing over 2021–2025, with no transaction costs

---

## Key Results

**Benchmark comparison (2016–2025, full period)**

| Metric | M7 Equal-Weight | S&P 500 | QQQ |
|---|---:|---:|---:|
| Cumulative return | +2,260.4% | +240.1% | +503.2% |
| Maximum drawdown | −49.39% | −33.92% | −35.12% |

**Individual stocks (2016–2025, annualized)**

| Stock | Return | Volatility | Beta | Sharpe | CAPM alpha |
|---|---:|---:|---:|---:|---:|
| AAPL | 28.67% | 29.05% | 1.208 | 0.815 | +12.88% |
| GOOG | 25.63% | 28.75% | 1.143 | 0.718 | +10.43% |
| META | 26.27% | 38.52% | 1.306 | 0.552 | +9.61% |
| AMZN | 25.23% | 32.80% | 1.175 | 0.617 | +9.75% |
| MSFT | 26.76% | 26.76% | 1.181 | 0.813 | +11.22% |
| NVDA | 67.19% | 49.89% | 1.800 | 1.247 | +46.12% |
| TSLA | 51.68% | 59.28% | 1.605 | 0.787 | +32.35% |

S&P 500 annualized return in this sample: 13.93%. Risk-free rate: 5.0%.

**Out-of-sample performance (2021–2025, weights fitted on 2016–2020)**

| Strategy | Annualized return | Volatility | Sharpe | Cumulative return | Max drawdown |
|---|---:|---:|---:|---:|---:|
| Max-Sharpe | 38.61% | 37.71% | **0.89** | **379.74%** | −54.00% |
| Min-Variance | 23.88% | 25.98% | 0.73 | 177.41% | −40.42% |
| Equal-Weight | 29.90% | 30.26% | 0.82 | 252.63% | −49.39% |
| S&P 500 | 13.80% | 16.96% | 0.52 | 84.98% | −25.43% |
| QQQ | 16.96% | 22.68% | 0.53 | 104.66% | −35.12% |

In-sample Sharpe for the max-Sharpe portfolio was 1.542 and fell to 0.891 out of sample.

**Optimized weights (fitted on 2016–2020, no position cap):** AAPL 18.65%, AMZN 18.90%, NVDA 36.62%, TSLA 25.83%; GOOG, META and MSFT receive zero weight.

**Portfolio risk (correlated Monte Carlo, 2021 starting prices, 1-year horizon)**

| Portfolio | Mean simulated return | 95% VaR | 95% CVaR |
|---|---:|---:|---:|
| Equal-Weight | 55.38% | −7.29% | −17.42% |
| Max-Sharpe | 80.55% | −5.57% | −17.08% |

The max-Sharpe portfolio's less-negative VaR mostly reflects its higher simulated mean return. Its CVaR is close to equal-weight's.

**Single-stock NVDA Monte Carlo (10,000 paths, full-history parameters, from $186.06):** median one-year price $321.36; 5th–95th percentile range $142.43–$721.57; 99% simulated VaR −46.05%.

**Rebalancing backtest (2021–2025, $100,000 start, no transaction costs):** monthly rebalancing finished at $349,472 (+249.5%); constant-weight daily rebalancing finished at $352,627 (+252.6%). The difference is small and is specific to this period.

---

## Major Findings

* **Concentration drives the historical return.** The M7 group outperformed both benchmarks over 2016–2025, but with a maximum drawdown about 15 points deeper than the S&P 500.
* **Mean-variance optimization did not beat equal-weight out of sample on a risk-adjusted basis.** Out-of-sample Sharpe was 0.89 for max-Sharpe vs 0.82 for equal-weight. Its in-sample Sharpe of 1.54 decayed substantially.
* **Diversification within the group is limited.** Every pairwise correlation is positive; the average is 0.541, with MSFT–GOOG highest (0.703) and TSLA–META lowest (0.335).
* **Returns are not normally distributed.** Normality is rejected for all seven stocks with fat tails, so the GBM simulations are treated as baseline scenario models that likely understate tail risk.
* **Historical alpha is sample-specific.** All seven stocks show positive CAPM alpha over 2016–2025, which likely reflects the retrospectively selected universe and omitted factors, not repeatable stock-picking skill.

---

## Limitations

* **Survivorship and selection bias:** the Magnificent 7 was defined with hindsight, so the historical returns overstate what an investor could have achieved in advance.
* **Daily rebalancing in the out-of-sample test:** fitted weights are applied to daily returns, which keeps the portfolio at constant weights. A buy-and-hold test would allow weights to drift. This is an implementation assumption and does not re-optimize on evaluation data.
* **No transaction costs, taxes or market impact** are modeled in any backtest.
* **Single five-year out-of-sample window:** the results describe one period and are not evidence of general performance.
* **GBM assumptions:** the simulations assume normal log returns with constant drift and volatility, which the normality tests reject.

---

## Tech Stack

Python, Pandas, NumPy, SciPy, Matplotlib, Seaborn, yFinance, Jupyter Notebook

---

## Contact

**Aditya Singh Shekhawat | IIT Indore — (Final Year)**

[![GitHub](https://img.shields.io/badge/GitHub-aditya1596-black)](https://github.com/aditya1596)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Aditya-blue)](https://www.linkedin.com/in/aditya-singh-shekhawat-934930288/)
