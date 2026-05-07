# Shield v1 — Frozen Model Identity

> **Important regulatory notice.** This repository documents an open-source
> quantitative research project. All performance figures presented are
> **Simulated Theoretical Performance** derived from historical backtesting
> with the model frozen on the date indicated below. They do not represent
> actual capital deployment. Simulated past performance is not a reliable
> indicator of future results. All returns are presented **net of modeled
> trading costs** (spread, modeled non-linear market impact, and a fixed
> per-trade fee) and gross of any subscription fees, taxes, or platform
> charges. This material is intended for sophisticated readers capable of
> independently evaluating its limitations and is not a solicitation for any
> investment service.

This document describes the public identity of the Shield v1 model as it was frozen for the open-source research project. All values below are immutable — any change to the model produces v2 (see [`methodology/version_policy.md`](methodology/version_policy.md)).

## Freeze metadata

- **Frozen at (UTC):** `2026-04-28T12:00:00Z`
- **Model SHA256:** `319dd27c712d2a66cc67db60aedd08b7d0078258dc9004d421a89ba418ee0051`
- **VecNormalize SHA256:** `fdac8be6366b319517bbc85f644ab5568ece2d01f609c5af637f9edf9eec4532`

## Training data period

`2015-01-02 to 2026-04-21`, with rolling multi-year training windows and out-of-sample test segments.

## Validation methodology

Out-of-sample validation with anti-leakage purge/embargo safeguards over a rolling multi-year window, plus a held-out final segment never seen during training. Detailed methodology (group structure, purge length, embargo length, window size, resampling parameters) is shared with verified institutional contacts via methodology request on [the-bou.com](https://the-bou.com).

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

## Architecture (high-level)

Shield v1 is a deep-learning portfolio allocator with multiple stacked risk-management layers. Architectural specifics — algorithm family, network topology, feature extractors, risk-layer implementations, hyperparameters — are operational details shared with verified institutional contacts via methodology request, so the public record commits to the model's identity-by-hash without disclosing replicable detail.

## Validation summary

The following summary metrics were computed from a single deterministic out-of-sample backtest over a 138-trading-day held-out window (2025-10-01 to 2026-04-21), evaluated against a block-bootstrap distribution of resampled trajectories.

| Metric | Headline value |
|---|---|
| Sharpe (rf=4%) | ~ 2 |
| Sortino (rf=4%) | ~ 3 |
| Total return | ~ 16% |
| Max drawdown | < 5% |
| Calmar | ~ 7 |

**Detailed values, full bootstrap confidence intervals, and the underlying daily-returns vector are shared with verified institutional contacts via methodology request on [the-bou.com](https://the-bou.com).**

**SPY benchmark over the same period**: +6.63% total return.

## Execution

- Rebalance frequency: daily
- Execution time: 15:30 ET

## Risk management layers (active)

Multiple risk-management layers are active in production. Their existence is part of the public commitment; their internal logic and parameters are operational details shared with verified institutional contacts via methodology request.

## Versioning policy

This model is frozen. Any future change to the training data, hyperparameters, architecture, feature engineering, or risk-layer logic produces v2 with a 30-day public announcement; v1 continues running in paper alongside v2 until the Minimum Track Record Length (MinTRL) criterion at 95% confidence is satisfied, typically 6-24 months. Full policy in [`methodology/version_policy.md`](methodology/version_policy.md).

---

This document is published for transparency only and does not constitute investment advice.
