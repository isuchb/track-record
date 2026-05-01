# Shield v1 — Open-Source Quantitative Research Project

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

This repository is the public, append-only research log for Shield v1, a frozen quantitative model that publishes daily portfolio weights before market execution. Every daily prediction is committed to this repository with a cryptographic timestamp before the corresponding orders are sent to the broker, so the entire history is independently auditable. The model itself is frozen — its checkpoint hash is published in [`model_hashes/shield_v1.json`](model_hashes/shield_v1.json) and any future change requires a new version (see [`methodology/version_policy.md`](methodology/version_policy.md)).

## How to verify a prediction

1. Download the prediction file for the day you want to verify: `daily_predictions/YYYY/MM/YYYY-MM-DD.json`.
2. Inspect the git commit timestamp on GitHub blame for that file. It must precede 15:30 ET on the same trading day — otherwise the prediction was published after market close and is not part of the track record.
3. Verify that the publishing model has not changed by comparing its SHA256 with the one declared in [`model_hashes/shield_v1.json`](model_hashes/shield_v1.json), following the manual procedure in [`methodology/integrity.md`](methodology/integrity.md).

## Live dashboard

A visual view of the equity curve, metrics, and historical signals is available at [the-bou.com/track-record](https://the-bou.com/track-record). <!-- CHANGE_ME_ON_REBRAND -->

## Model identity

See [`SHIELD_V1_FROZEN.md`](SHIELD_V1_FROZEN.md) for the full description of what Shield v1 is, the universe it trades, and the public hyperparameters.

## Versioning policy

See [`methodology/version_policy.md`](methodology/version_policy.md) for the public commitment about when and how a new version may be released.

## Methodology

- [`methodology/overview.md`](methodology/overview.md) — algorithmic and validation philosophy.
- [`methodology/version_policy.md`](methodology/version_policy.md) — when and how a new version is released.
- [`methodology/integrity.md`](methodology/integrity.md) — what is published, what is private, and how to verify it.
- [`methodology/retirement_criteria.md`](methodology/retirement_criteria.md) — pre-committed quantitative and structural triggers under which Shield v1 will be retired.
- [`methodology/crisis_playbook.md`](methodology/crisis_playbook.md) — bi-phasic public communication protocol activated when any retirement criterion is triggered.

## Regulatory positioning

This research project is structured as an unregulated publisher of quantitative financial research, NOT as an investment advisor. To maintain this regulatory positioning under both EU MiFID II (CNMV interpretation) and US SEC Investment Advisers Act of 1940 (Publisher's Exclusion under Lowe v. SEC), the following operational guardrails apply:

1. **Zero personalization**: signals are identical for every subscriber. No subscriber profiling, no risk tolerance assessment, no investment goal questionnaires (no KYC/MiFID II suitability tests).
2. **Zero individualized communication**: the operator does not respond to subscriber emails seeking personalized guidance ("should I allocate X% to this signal?", "is this right for my situation?"). Such interactions would void the publisher exemption.
3. **Strict regular schedule**: signals are published on a fixed automated schedule (15:30 ET each trading day) to satisfy the SEC's "general and regular circulation" requirement under the Publisher's Exclusion.
4. **Marketing integrity**: this project is positioned exclusively as broad quantitative research, not personalized wealth management. Marketing copy on subscription pages, GitHub README, and all external communications consistently reflects this positioning.

A subscriber retains full responsibility for evaluating whether and how to act on the published signals. The operator does not provide investment advice in any jurisdiction.

## Contact

Reach out at [admin@the-bou.com](mailto:contact@the-bou.com). <!-- CHANGE_ME_ON_REBRAND -->

## Disclaimer

This repository is published for research transparency only. It does not constitute investment advice, an offer to provide investment services, or a solicitation. Simulated past performance does not guarantee future results.
