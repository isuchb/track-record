# Methodology — Retirement Criteria for Shield v1

## Purpose

This document is a public, pre-committed statement of the conditions under which Shield v1 will be retired and replaced. Publishing these criteria in advance — before any of them are tripped — is intended to neutralize **continuity bias**: the temptation, after the fact, to keep a degraded model in production because retiring it would damage the published track record. By writing the triggers down here, in advance, the decision becomes mechanical.

This document is independent of [`version_policy.md`](version_policy.md). The version policy describes how a new version is released and announced; this document describes when v1 itself becomes ineligible to keep operating, regardless of whether v2 is ready.

## Quantitative triggers

Any **one** of the following conditions, observed on live execution data, triggers retirement:

- **Maximum Drawdown threshold (primary failsafe)**: rolling MaxDD exceeds **20.6%** (2.0× the bootstrap-CI95 upper bound from validation, aligning with the institutional 2-sigma standard for manager replacement). This is the absolute final failsafe; complementary leading-indicator metrics below should trigger investigation well before this point is reached.
- **Modified Conditional Expected Drawdown (MCED)**: as defined by Goldberg & Mahmoud (2014), MCED smooths the path-dependence of MaxDD and is more robust to sampling error. Trigger investigation if MCED exceeds 1.5x its in-sample bootstrap mean for 30 consecutive days.
- **Rolling Calmar deterioration**: 6-month rolling Calmar ratio drops below 0.5 (vs deterministic backtest of 7.34). Calmar collapse is a leading indicator that recovery efficiency is failing before MaxDD limit is breached.
- **High-Water Mark (HWM) duration**: portfolio remains under its prior HWM for more than 90 consecutive trading days, indicating structural alpha decay or regime mismatch.
- **Sharpe collapse.** The rolling 90-day annualized Sharpe ratio drops below 0.5 and remains below 0.5 for 60 consecutive trading days.
- **Tracking error vs published prediction stream**: cumulative tracking error exceeds 100 bps over any 30-day window, indicating execution drift between published signal and actual broker fills.

## Structural triggers

Any **one** of the following conditions also triggers retirement, independently of any quantitative metric:

- A code-level bug discovered anywhere in the live-inference pipeline that affects published predictions, even if the impact on metrics is small.
- Any change to upstream data sources (Alpaca market data, FRED macro series, yfinance auxiliary feeds) that materially shifts the input feature distribution beyond the range observed during training.
- Any regulatory change that prohibits the current operating model — for example, a binding interpretation that would forbid publishing pre-trade weights as research.

## Process upon trigger

When any of the above triggers fires:

1. A public retirement notice is posted to this repository within 7 days, identifying the trigger and the date it fired.
2. A post-mortem report is published within 30 days at `monthly_reports/post_mortem_shield_v1.md`. The post-mortem describes the trigger event, the root cause, the impact on the track record, and the lessons carried into v2.
3. If the trigger is structural (bug, regulatory, data source change), the successor is launched as v2 (major) following the standard major-release protocol with the 30-day announcement WAIVED due to the emergency.
4. v1 itself continues running in paper trading after retirement, exactly as if v2 had launched normally, until the MinTRL criterion described in [`version_policy.md`](version_policy.md) is satisfied at 95% confidence, so the audit trail is not interrupted.
5. See [crisis_playbook.md](crisis_playbook.md) for the bi-phasic public communication protocol.

## What is NOT a trigger

To avoid hair-trigger retirement on noise, the following situations are explicitly **not** triggers:

- A single bad month that stays inside the bootstrap 95 percent CI on monthly returns.
- A missed prediction in the sense of "the signal was published on time and acted upon, but it underperformed". The contract is to publish a faithful daily signal, not to be right every day.
- A routine drawdown that remains within the bootstrap CI95 band on Maximum Drawdown from validation.

## Relationship to versioning policy

Retirement triggers ALWAYS produce a major version bump (v(n) → v(n+1)). They never produce minor or patch versions, because by definition the trigger indicates the existing model is no longer fit for purpose. The 30-day pre-announcement window required for standard major releases is waived when retirement is triggered, replaced by an immediate retirement notice followed by a post-mortem within 30 days. The successor v2 then follows its standard launch protocol once ready.

---

This document is published for research transparency only and does not constitute investment advice. Simulated past performance does not guarantee future results.
