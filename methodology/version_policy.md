# Methodology — Version Policy

This is the public commitment about when and how Shield can be updated. Its purpose is to make the track record auditable across time: a reader who sees a one-year track record must know what kind of changes (if any) the underlying model has undergone in that year, and which changes were severe enough to require a fresh track record.

Shield uses a three-tier semver-style versioning scheme: **patch / minor / major**. The tier determines whether the track record continues unbroken, continues with annotation, or resets to zero, and whether a public announcement window is required before the change ships.

## Patch versions (v1.0.0 → v1.0.1)

**Track record continuity**: 100%, no announcement, no parallel run.

Triggers:

- Bug fixes in code that produce bit-identical predictions on the same input
- Logging, monitoring, deployment infrastructure changes
- Documentation updates
- Optimizations verified via regression testing on a fixed input set

Documented in CHANGELOG entries pushed to the repository. The model SHA256 may change if the bug fix touched code that loads/serializes the model, but predictions on identical input must remain bit-identical (verified by test suite).

## Minor versions (v1.0 → v1.1)

**Track record continuity**: continues with annotation. Equity curve labels the boundary date as "v1.0 → v1.1 transition". No reset to zero. No mandatory parallel run, but a **30-day announcement** is required.

Triggers:

- Cost model recalibration (e.g., Almgren-Chriss eta updated to match empirical live slippage)
- Risk-layer parameter recalibration (target_vol_daily, max_drawdown_limit, per_asset_cap) within the original mandate
- Retraining with newer market data using the SAME architecture, SAME hyperparameters, SAME feature set (a "rolling window forward")
- Adding non-decision-changing infrastructure (better data validation, tighter monitoring)

The model SHA256 will change. The minor version SHA256 is published in the same `model_hashes/` directory under `shield_v1_1.json` (etc.). Track record continues unbroken.

## Major versions (v1 → v2)

**Track record continuity**: v2 starts a fresh track record from zero. v1 keeps running in parallel paper trading until the MinTRL criterion below is satisfied. Mandatory **30-day public announcement** before v2 launches.

**Parallel-run duration is dynamic, not calendar-based.** v1 keeps running in parallel with v2 until the difference in their out-of-sample Sharpe ratios achieves 95% statistical significance, computed continuously via the Minimum Track Record Length (MinTRL) framework of Bailey & López de Prado (2014). The MinTRL formula factors in the observed Sharpe difference, the skewness, and the kurtosis of the actual live returns:

```
MinTRL = (V_a + V_b - 2·Cov(a,b)) · (z_{1-α} / SR_diff)^2
```

Where V_a, V_b are the sample variances of the v1 and v2 Sharpe estimators, Cov(a,b) is their covariance, z_{1-α} is the critical value for the desired confidence (1.645 for 95%), and SR_diff is the observed difference. In practice the parallel run will last anywhere from 6 months (if v2 outperforms with low volatility and normal kurtosis) to 24 months (if outperformance is marginal or returns are heavy-tailed). The exact end date cannot be promised in advance to subscribers — only the statistical criterion that triggers the transition.

Triggers:

- Architecture changes (network shape, feature extractors, attention modules, network width/depth)
- Algorithm changes (e.g., TQC → distributional SAC, distributional IQN, etc.)
- Feature engineering changes (new inputs, dropped inputs, transformed inputs)
- Risk-management layer changes (adding, removing, or modifying the LOGIC of any layer)
- Universe changes (different ETFs, different number of ETFs)
- Validation methodology changes (CPCV → walk-forward, different purge/embargo)

Both v1 and v2 run concurrently in paper trading for the dynamic MinTRL window described above (typically 6-24 months depending on observed Sharpe difference, skewness, and kurtosis). The parallel-run period generates a side-by-side performance comparison published in `monthly_reports/v1_v2_parallel_run_YYYY-MM.md`. Subscribers and external auditors can independently verify whether v2 outperforms v1 on the SAME live trading days — this is the strongest form of evidence that v2 represents a genuine improvement, not retrofitting to past data.

## Decision tree (when which)

- "Does the change affect predicted weights on identical inputs?" → No → PATCH; Yes → continue
- "Does the change touch the network architecture, feature set, algorithm, risk-layer logic, or universe?" → Yes → MAJOR; No → MINOR
- "If MINOR: are the changes within the original mandate?" → Yes → MINOR (v1.x); No → MAJOR (v2)
- "If MAJOR: is the change driven by retirement criteria triggering?" → Yes → emergency major (no 30-day window, immediate retirement notice + post-mortem first); No → standard major (30-day announcement)
- "If MAJOR: when does the parallel run end?" → When MinTRL is satisfied at 95% confidence, NOT at a fixed calendar date.

## Public artifacts

All checkpoint hashes and commit timestamps are public from day one of each version. There is no private window. Announcements are posted to [the-bou.com](https://the-bou.com) <!-- CHANGE_ME_ON_REBRAND --> and the public Substack newsletter. No silent rollouts.

## See also

- [`retirement_criteria.md`](retirement_criteria.md) — pre-committed quantitative and structural triggers under which v1 will be retired (independently of, and possibly before, any v2 release). Retirement triggers always produce a major version bump and waive the 30-day pre-announcement window.

---

This document is published for research transparency only and does not constitute investment advice.
