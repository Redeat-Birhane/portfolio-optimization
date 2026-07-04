# Time Series Forecasting for Portfolio Management Optimization

## Overview

This project applies time series forecasting to historical financial data to support
portfolio management decisions for **Guide Me in Finance (GMF) Investments**, a
financial advisory firm specializing in personalized, data-driven investment
strategies.

Acting as a Financial Analyst, this project builds forecasting models for
**Tesla (TSLA)** — a high-growth, high-volatility stock — and combines that
forecast with historical data for **BND** (a low-risk bond ETF) and **SPY**
(a moderate-risk S&P 500 ETF) to construct and validate an optimized portfolio
using Modern Portfolio Theory.

The analysis covers the period **January 1, 2015 – June 30, 2026**, using data
sourced from the [YFinance](https://pypi.org/project/yfinance/) Python library.

> **Note:** In line with the Efficient Market Hypothesis, this project does not
> assume exact future stock prices can be reliably predicted from historical
> price data alone. Forecasts here are used as one input among several —
> alongside volatility analysis, risk metrics, and portfolio theory — to support
> a broader asset allocation decision, not as a standalone trading signal.

## Project Structure
portfolio-optimization/
├── .vscode/                # Editor settings
├── .github/workflows/      # CI configuration (unit tests run on push/PR)
├── data/
│   └── processed/          # Cleaned, model-ready datasets (output of notebooks/1.0-eda.ipynb)
├── notebooks/               # Exploratory and modeling notebooks (EDA, ARIMA, LSTM, forecasting, portfolio optimization, backtesting)
├── src/                     # Reusable modules: data loading, train/test splitting, evaluation metrics, plotting utilities
├── tests/                   # Unit tests for functions in src/
├── scripts/                 # Standalone scripts (e.g., automated data refresh, batch runs)
├── requirements.txt         # Python dependencies
└── README.md

**Folder purposes:**
- `data/processed/` — stores cleaned datasets produced by the EDA notebook (e.g., `combined_assets.csv`), so downstream notebooks don't need to re-fetch or re-clean raw data from YFinance.
- `notebooks/` — the analytical narrative: EDA, model training, forecasting, and portfolio optimization, each importing shared logic from `src/`.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Redeat-Birhane/portfolio-optimization.git
cd portfolio-optimization
```

### 2. Create and activate a virtual environment

```bash
python -m venv venv

# On macOS/Linux
source venv/bin/activate

# On Windows
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

Then open the notebooks in `notebooks/` in the order described in the pipeline below.

## Pipeline

The project is organized as a sequential pipeline. Each stage builds on the
output of the previous one, so notebooks should be run in order.

| Stage | Notebook | Description |
|-------|----------|-------------|
| **1. Data Preprocessing & EDA** | `1.0-eda.ipynb` | Downloads TSLA, BND, and SPY data via YFinance; cleans missing values; analyzes trends, volatility, outliers, and stationarity (ADF test); calculates VaR and Sharpe Ratio; saves cleaned data to `data/processed/`. |
| **2. Forecasting Models** | `2.0-modeling.ipynb` | Splits data chronologically; builds and compares ARIMA/SARIMA and LSTM models for TSLA; evaluates with MAE, RMSE, MAPE; selects the best-performing model. |
| **3. Future Forecasting** | `3.0-forecasting.ipynb` | Generates a 6–12 month forward forecast using the best model from Stage 2, with confidence intervals; analyzes long-term trend, opportunities, and risks. |
| **4. Portfolio Optimization** | `4.0-portfolio-optimization.ipynb` | Combines the TSLA forecast with historical average returns for BND/SPY; computes the covariance matrix; generates the Efficient Frontier; identifies the Max Sharpe Ratio and Min Volatility portfolios; recommends a final allocation. |
| **5. Strategy Backtesting** | `5.0-backtesting.ipynb` | Simulates the recommended portfolio over the most recent year of data against a static 60% SPY / 40% BND benchmark; compares cumulative returns, Sharpe Ratio, and max drawdown. |

**High-level flow:**
YFinance Data → Clean & Explore → Forecast TSLA (ARIMA vs LSTM)
→ Generate Future Forecast → Optimize Portfolio (MPT)
→ Backtest Strategy vs Benchmark

## Requirements

See `requirements.txt` for the full list. Core dependencies include:
`yfinance`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`,
`pmdarima`, `scikit-learn`, `tensorflow`, `PyPortfolioOpt`, `scipy`.