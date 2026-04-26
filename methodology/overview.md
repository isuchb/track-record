# Methodology — Overview

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

## What Shield is

Shield is a distributional reinforcement-learning portfolio manager developed as an open-source quantitative research project. The agent allocates daily weights across a fixed universe of 25 defensive and macro ETFs (see [`../SHIELD_V1_FROZEN.md`](../SHIELD_V1_FROZEN.md) for the full list). The decision policy is trained with TQC (Truncated Quantile Critics), a distributional RL algorithm that learns the full return distribution of each action, not just its expected value.

## Why TQC

Distributional RL methods learn quantiles of the return distribution, which makes left-tail (downside) risk an explicit, first-class signal during training. Point-estimate methods such as DQN or PPO collapse the return distribution to its mean and lose this information. TQC additionally truncates the top quantiles of each critic, which biases the value estimate downward and reduces overestimation in volatile environments — a common pathology when value-based RL is applied to financial markets.

## Why CPCV

Standard walk-forward validation produces a single OOS path. Combinatorial Purged Cross-Validation (CPCV) splits the data into N groups, holds out k of them as test set in every combination, and rotates over all C(N, k) splits. Purging removes training observations whose labels overlap the test window, and embargoing leaves a buffer after each test block to prevent serial-correlation leakage. The result is multiple OOS paths instead of one, which mitigates selection bias on the validation metric.

## Validation hierarchy

Shield v1 reports results from two distinct validation regimes, and they do not have equal statistical weight.

1. **CPCV in-sample (primary evidence).** With 6 groups and 2 test groups per combination, CPCV produces 15 distinct OOS paths (`C(6,2) = 15`). Aggregating performance across 15 paths instead of one substantially mitigates the selection bias that plagues single-split validation, and is the primary source of evidence for the model's behavior across regimes. This is the regime against which the bootstrap confidence intervals (see [`integrity.md`](integrity.md)) are computed.

2. **Hold-out OOS Oct-2025 to Apr-2026 (sanity check).** A 138-trading-day post-CPCV hold-out window is reserved as a final out-of-sample check. Because it is a single contiguous path it carries less statistical weight than the 15 CPCV paths and is treated as a sanity check rather than primary evidence. A passing CPCV that fails the hold-out is a red flag; a passing hold-out that fails CPCV is not enough to publish the model.

3. **Probability of Backtest Overfitting (PBO).** PBO is computed following López de Prado (2014) over the matrix of CPCV path performances. The published value will appear here once the full CPCV CSV is processed: `<PBO_PLACEHOLDER>`. A PBO above 0.5 indicates that the in-sample selection has more than even odds of being overfit and would invalidate publication of the model.

## Drawdown-first philosophy

Shield's reward shaping treats drawdown as the primary objective and total return as a secondary objective subject to a return floor. This is enforced jointly through several mechanisms (CBF-QP projection, RCPO duals, drawdown-aware reward shaping, vol-targeting overlay) rather than through a single penalty term. The intent is a portfolio that loses less in regime transitions, at the cost of capping upside in trending bull markets.

## Out of scope for this document

The exact set of input features, the internal logic of the risk-management layers, and the full hyperparameter set used to train Shield v1 are not part of the public methodology. Only the presence/absence of each risk layer is disclosed in [`../SHIELD_V1_FROZEN.md`](../SHIELD_V1_FROZEN.md). The public artifact is the frozen checkpoint hash and the daily prediction history.

---

This document is published for research transparency only and does not constitute investment advice. Simulated past performance does not guarantee future results.
