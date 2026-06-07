#  Magnificent 7 Portfolio Optimization & Risk Forecasting

An end-to-end quantitative finance project analyzing the **Magnificent 7 stocks (AAPL, GOOG, META, AMZN, NFLX, NVDA, TSLA)** using portfolio theory, statistical analysis, and Monte Carlo simulation to evaluate performance, optimize allocation, and forecast downside risk.

---

##  Highlights

* Achieved approximately **250% cumulative portfolio return** over 5 years compared to **~80% for the S&P 500** and **~127% for QQQ**
* Constructed the **Efficient Frontier** using **20,000 simulated portfolios**
* Performed **Monte Carlo simulations (10,000 paths)** for portfolio and stock-level risk forecasting
* Reduced portfolio downside risk from **~−31% VaR to ~−27% VaR** through optimization
* Applied CAPM, Alpha analysis, Beta estimation, Sharpe Ratio evaluation, and portfolio rebalancing strategies

---

##  Project Objective

Most stock analyses focus only on returns. This project evaluates investments through a broader quantitative framework that incorporates:

* Return
* Risk
* Diversification
* Portfolio Optimization
* Downside Risk
* Future Uncertainty

The goal is to determine whether the Magnificent 7 can deliver superior risk-adjusted performance and how capital should be allocated across them.

---

##  Dataset

| Parameter    | Details                                  |
| ------------ | ---------------------------------------- |
| Source       | Yahoo Finance (`yfinance`)               |
| Stocks       | AAPL, GOOG, META, AMZN, NFLX, NVDA, TSLA |
| Benchmarks   | S&P 500 (`^GSPC`), QQQ ETF               |
| Time Horizon | 5 Years                                  |
| Frequency    | Daily Closing Prices                     |

The selected period captures multiple market environments, including the COVID crash, post-pandemic recovery, rate-hike driven correction, and AI-led technology rally.

---

## 🔍 Analysis Pipeline

### 1. Exploratory Data Analysis

* Historical price trends
* Normalized growth comparison (Base = 100)
* SMA vs EMA trend analysis
* Daily return distributions

### 2. Statistical Analysis

* Daily return calculation
* D’Agostino-Pearson normality testing
* Volatility analysis
* Correlation heatmap

### 3. Risk & Performance Metrics

* Annualized Return
* Annualized Volatility
* Beta
* Sharpe Ratio
* CAPM Expected Return
* Alpha Analysis

### 4. Benchmark Comparison

Performance comparison against:

* S&P 500
* QQQ ETF

Additional evaluation:

* Excess Return (Alpha)
* Maximum Drawdown

### 5. Portfolio Optimization

Using Modern Portfolio Theory (Markowitz Framework):

* Simulated **20,000 random portfolios**
* Calculated Return, Volatility and Sharpe Ratio
* Constructed Efficient Frontier
* Selected Maximum Sharpe Ratio Portfolio

### 6. Monte Carlo Risk Forecasting

* Geometric Brownian Motion (GBM)
* Cholesky Decomposition for correlated simulations
* **10,000 future portfolio scenarios**
* Individual NVDA price-path simulation

### 7. Value at Risk (VaR)

* Portfolio 95% VaR
* Individual NVDA 99% VaR
* Downside risk comparison between Equal-Weight and Optimized portfolios

### 8. Portfolio Backtesting

Comparison of:

* Buy & Hold
* Monthly Rebalancing

to evaluate diversification benefits and long-term performance.

---

##  Key Results

| Metric                         | Result |
| ------------------------------ | ------ |
| M7 Portfolio Return            | ~250%  |
| S&P 500 Return                 | ~80%   |
| QQQ Return                     | ~127%  |
| Portfolios Simulated           | 20,000 |
| Monte Carlo Paths              | 10,000 |
| Optimal Portfolio Sharpe Ratio | ~0.86  |
| Equal-Weight Portfolio VaR     | ~−31%  |
| Optimized Portfolio VaR        | ~−27%  |
| NVDA 99% VaR                   | ~−7%   |

---

##  Major Findings

* NVIDIA was the strongest performer across most return-based metrics during the study period.
* Most stock returns rejected the normality assumption, highlighting the presence of fat-tail risk in financial markets.
* The Magnificent 7 portfolio significantly outperformed both the S&P 500 and QQQ benchmarks.
* Portfolio optimization improved risk-adjusted performance while reducing downside exposure.
* Monthly rebalancing improved diversification but slightly reduced returns by trimming strong-performing stocks during bullish periods.
* Positive correlations across all seven stocks indicate limited diversification within the group alone.

---

##  Tech Stack

* Python
* Pandas
* NumPy
* SciPy
* Matplotlib
* Seaborn
* yFinance
* Jupyter Notebook

---

##  Limitations

* Survivorship bias due to focusing on the Magnificent 7
* Historical performance may not represent future outcomes
* No transaction costs included in rebalancing analysis
* Monte Carlo simulations assume constant drift and volatility
* Portfolio results depend on the selected historical period

---

---

## Contact

**Aditya Singh Shekhawat | IIT Indore — Civil Engineering (Final Year)**

[![GitHub](https://img.shields.io/badge/GitHub-aditya1596-black)](https://github.com/aditya1596)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Aditya-blue)](https://www.linkedin.com/in/aditya-singh-shekhawat-934930288/)