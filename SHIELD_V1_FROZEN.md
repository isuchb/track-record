# Shield v1 — Frozen Model Identity

> **Important regulatory notice.** This repository documents an open-source
> quantitative research project. All performance figures presented are
> **Simulated Theoretical Performance** derived from historical backtesting
> with the model frozen on the date indicated below. They do not represent
> actual capital deployment. Simulated past performance is not a reliable
> indicator of future results. All returns are presented **net of modeled
> trading costs** (10 bps buy + 10 bps sell + Almgren-Chriss non-linear market
> impact + $0.01 fixed fee per trade) and gross of any subscription fees,
> taxes, or platform charges. This material is intended for sophisticated
> readers capable of independently evaluating its limitations and is not a
> solicitation for any investment service.

This document describes the identity of the Shield v1 model as it was frozen for the open-source research project. All values below are immutable — any change to the model produces v2 (see [`methodology/version_policy.md`](methodology/version_policy.md)).

## Freeze metadata

- **Frozen at (UTC):** `<TO BE FILLED AT FREEZE CEREMONY>`
- **Model SHA256:** `<TO BE FILLED AT FREEZE CEREMONY>`
- **VecNormalize SHA256:** `<TO BE FILLED AT FREEZE CEREMONY>`

## Training data period

`2015-01-02 to 2026-04-21 (each CPCV combination uses a rolling 4-year training window)`

## Validation methodology

Combinatorial Purged Cross-Validation (CPCV) with 6 groups, 2 test groups per combination, 21-day purge, 20-day embargo, and a rolling 4-year window. See [`methodology/overview.md`](methodology/overview.md) for the rationale.

## Universe (25 ETFs)

- TLT — iShares 20+ Year Treasury Bond ETF
- IEF — iShares 7-10 Year Treasury Bond ETF
- SHV — iShares Short Treasury Bond ETF
- HYG — iShares iBoxx High Yield Corporate Bond ETF
- LQD — iShares iBoxx Investment Grade Corporate Bond ETF
- MUB — iShares National Muni Bond ETF
- TIP — iShares TIPS Bond ETF
- GLD — SPDR Gold Shares
- SLV — iShares Silver Trust
- GDX — VanEck Gold Miners ETF
- DBC — Invesco DB Commodity Index Tracking Fund
- USO — United States Oil Fund
- DBA — Invesco DB Agriculture Fund
- UUP — Invesco DB US Dollar Index Bullish Fund
- FXF — Invesco CurrencyShares Swiss Franc Trust
- FXY — Invesco CurrencyShares Japanese Yen Trust
- FXE — Invesco CurrencyShares Euro Trust
- XLP — Consumer Staples Select Sector SPDR Fund
- XLU — Utilities Select Sector SPDR Fund
- XLV — Health Care Select Sector SPDR Fund
- USMV — iShares MSCI USA Min Vol Factor ETF
- SPY — SPDR S&P 500 ETF
- QQQ — Invesco QQQ Trust
- BNO — United States Brent Oil Fund
- CPER — United States Copper Index Fund

## Public hyperparameters

- Algorithm: TQC (Truncated Quantile Critics)
- Critics: 5
- Quantiles per critic: 25
- Top quantiles dropped per net: 2
- net_arch: [256, 256] over feature extractor
- frame_stack: 15
- gamma: 0.98
- learning_rate: 3e-5
- batch_size: 512
- Feature extractors: Conv1D, Transformer, CAAN (d_per_stock=8), Macro Attention (macro_dim=64)

> Note: Shield v1 was trained with a deliberate trade-off favoring time-to-market: replay buffer size of 100,000 transitions and 10 CPCV combinations (vs the more conservative 200,000+ buffer and 20-30 combinations recommended for higher statistical confidence). Successor version v2 will increase these to the conservative range. This trade-off is documented for radical transparency.

## Validation metrics (Stationary Block Bootstrap)

The following metrics were computed from a single deterministic out-of-sample backtest over 138 trading days (2025-10-01 to 2026-04-21). Confidence intervals are derived via 10,000 stationary block bootstrap resamples (Politis & Romano 1994), with block length calibrated from the lag-1 autocorrelation of squared returns (mean block length ≈ 2 days, reflecting low volatility clustering in the period).

| Metric | Deterministic | Bootstrap median | 95% CI low | 95% CI high | Std |
|---|---|---|---|---|---|
| Sharpe (rf=4%) | 2.34 | 2.37 | -0.37 | 5.49 | 1.50 |
| Sortino (rf=4%) | 2.80 | 2.95 | -0.42 | 8.86 | 2.38 |
| Total return | 16.12% | 16.14% | -0.58% | 34.05% | 8.78% |
| Max drawdown | 4.28% | 4.58% | 2.04% | 10.32% | 2.11% |
| Calmar | 7.34 | 6.79 | -0.12 | 29.37 | 7.77 |

**Reading the confidence intervals**: a CI95 spanning negative values does NOT indicate the strategy is broken. It reflects the limited sample size of 138 trading days. With more live execution data accumulating after the freeze date, these intervals will tighten substantially. The bootstrap median being close to the deterministic path indicates the deterministic backtest is representative of the underlying distribution rather than an outlier.

**SPY benchmark over the same period**: +6.63% total return.
**Beta vs SPY**: <PENDING_CALCULATION> (separate computation, not in current bootstrap output).

## Execution

- Rebalance frequency: daily
- Execution time: 15:30 ET

## Risk management layers (active)

The following risk layers are active in production. Their internal logic is not public; only the on/off status of each layer is disclosed.

- CBF-QP (Control Barrier Function via Quadratic Programming)
- BOCPD (Bayesian Online Changepoint Detection)
- RCPO (Reward Constrained Policy Optimization)
- ALM (Augmented Lagrangian Method)
- Vol-targeting (dual EWMA 20/60)
- TQC truncation annealing
- Drawdown-aware reward shaping

## Versioning policy

This model is frozen. Any future change to the training data, hyperparameters, architecture, feature engineering, or risk layer logic produces v2 with a 30-day public announcement; v1 continues running in paper alongside v2 until the Minimum Track Record Length (MinTRL) criterion at 95% confidence is satisfied (Bailey & López de Prado 2014), typically 6-24 months. Full policy in [`methodology/version_policy.md`](methodology/version_policy.md).

---

This document is published for transparency only and does not constitute investment advice.
