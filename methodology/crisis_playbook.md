# Crisis Communication Playbook

This document defines the public communication protocol activated when any retirement criterion is triggered. Pre-drafted during a period of calm, designed to project institutional maturity rather than panic.

> **Important regulatory notice.** This document is part of an open-source quantitative research project. It does not constitute investment advice or a solicitation. Simulated past performance is not a reliable indicator of future results.

## Phase 1: Immediate notification (T+0)

Upon any retirement criterion being breached, an automated notice is dispatched to all subscribers and pushed to this repository within 1 hour. The notice contains exclusively factual parameters with zero conjecture or defensive posturing:

- Status confirmation: explicit acknowledgment of which threshold was breached and at what value
- Operational action: confirmation that signal generation and paper-trading execution have been suspended in accordance with the pre-committed framework
- Timeline commitment: a firm commitment to deliver a comprehensive technical post-mortem within 72 hours

## Phase 2: Technical post-mortem (T+72h)

Exactly 72 hours after the immediate notification, a complete forensic analysis is published openly to this repository (no paywall). Hiding model failures behind a paywall generates suspicion. The post-mortem must address:

1. **Attribution of failure**: was the drawdown caused by an exogenous macroeconomic shock not represented in training data, or by a systemic algorithmic failure (e.g., critic Q-value mis-estimation, regime detection error, vol-targeting overshoot)?

2. **Infrastructure integrity check**: explicit confirmation that AWS Fargate execution, Alpaca API routing, S3 data pipelines, and the GitHub publish loop functioned correctly. This isolates the failure to alpha decay rather than engineering incompetence.

3. **Re-engineering roadmap**: actionable steps detailing how the insights from this failure will be incorporated into the v(n+1) successor model.

## Why this protocol exists

Historical precedents inform the design:

- **Amaranth Advisors (2006)** prioritized internal capital preservation over investor communication, dispatching its first letter only after the collapse was complete. The lack of proactive transparency triggered mass panic and a fire-sale liquidation.
- **Long-Term Capital Management (1998)** maintained opacity regarding leverage and failing models until federal intervention was required.
- **Bridgewater Associates (2020 COVID drawdown)** maintained institutional trust through radical transparency: detailed letters explaining the macroeconomic paradigm shift, the precise mechanical reasons for the drawdown, and the structural adjustments being implemented.

The Bridgewater model, not the Amaranth model, is followed here.

---

This document is part of an open-source quantitative research project and does not constitute investment advice.
