# Monte Carlo VaR and Expected Shortfall

Python implementation for estimating **Value at Risk (VaR)** and **Expected Shortfall (CVaR)** on a single equity position using historical, parametric, and Monte Carlo methods.

## Overview

This project analyzes Apple (AAPL) daily returns and computes 95% one-day risk metrics through:

| Method | Description |
|--------|-------------|
| Historical | Empirical quantiles from observed returns |
| Parametric | Normal distribution fitted to sample mean and volatility |
| Monte Carlo | Geometric Brownian Motion with 1,000 simulated paths |

The notebook also scales VaR across holding periods using the square-root-of-time rule and compares tail-risk estimates across approaches.

## Repository structure

```
monte-carlo-var-cvar/
├── notebooks/
│   └── monte_carlo_var_cvar.ipynb   # Main analysis notebook
├── requirements.txt
└── README.md
```

## Requirements

- Python 3.10+
- See `requirements.txt` for package versions

## Setup

```bash
git clone https://github.com/kiran3035/monte-carlo-var-cvar.git
cd monte-carlo-var-cvar
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
jupyter notebook notebooks/monte_carlo_var_cvar.ipynb
```

## Methodology (summary)

1. **Data** — Download adjusted close prices via `yfinance` (default: 4 years of AAPL).
2. **Returns** — Compute simple daily returns; inspect distribution, skewness, and kurtosis.
3. **Historical VaR / CVaR** — 5th percentile of returns; CVaR as the mean of returns below VaR.
4. **Parametric VaR / CVaR** — `scipy.stats.norm.ppf` at the 5% tail; CVaR from empirical tail mean.
5. **Time scaling** — `VaR_t = VaR_1d × √t` for horizons up to 252 trading days.
6. **Monte Carlo** — Simulate log-normal daily returns; estimate VaR and CVaR from the simulated distribution.

## Key outputs (example run)

Typical 95% one-day results for AAPL over a multi-year sample:

| Metric | Historical | Parametric | Monte Carlo |
|--------|------------|------------|-------------|
| VaR | ~−2.7% | ~−2.7% | ~−2.9% |
| CVaR | ~−3.8% | ~−3.9% | ~−3.5% |

Exact values depend on the download date and market window.

## Notes

- VaR and CVaR are reported as **percentage returns** (negative = loss).
- Returns are not perfectly normal (fat tails); historical CVaR often exceeds Monte Carlo CVaR.


## License

MIT
