# FYOS Open Methodology

This repository documents the public methodology behind [FYOS](https://fyos.pro) — a funding rate intelligence platform for crypto perpetual futures.

## What This Repository Contains

FYOS uses a quantitative methodology to transform raw funding rate data into actionable intelligence. This repository explains the core concepts, formulas, and design principles — without exposing production implementation details.

**This is methodology documentation, not production code.**

## Core Philosophy

### The Mirage Problem

Most funding rate analytics present **gross annualized rates** as if they represent achievable returns. In practice:

- Trading fees consume 30-60% of gross yield
- Rates mean-revert, often reversing before positions can be established
- High headline rates frequently signal crowded trades with adverse execution
- Slippage at scale can exceed the entire expected return

FYOS exists because the gap between "headline" yields and "deployable" yields is often 50-90%. We call this the **Mirage Problem**.

### Trust Over Hype

Every metric FYOS produces carries explicit uncertainty disclosure:

- **Empirical validation**: We track prediction error against realized outcomes
- **Conservative haircuts**: Displayed yields include deductions for historical forecast error
- **Regime awareness**: Metrics account for funding rate persistence and volatility clustering
- **Capacity constraints**: We show position sizes where edge degrades, not infinite scalability

## Methodology Topics

| Topic | Description |
|-------|-------------|
| [APR Progression](methodology/01-apr-progression.md) | From gross rate to deployable yield |
| [Funding Persistence](methodology/02-funding-persistence.md) | Measuring regime stability |
| [Mirage Detection](methodology/03-mirage-detection.md) | Identifying unsustainable rates |
| [Reality Gap](methodology/04-reality-gap.md) | Historical vs. recent rate divergence |
| [Deterministic Decay](methodology/05-deterministic-decay.md) | How rates compress over time and what persists |
| [Trust Grading](methodology/06-trust-grading.md) | Empirical reliability calibration |
| [Capacity Modeling](methodology/07-capacity-modeling.md) | Position size constraints |
| [Edge Value](methodology/08-edge-value.md) | Holistic opportunity scoring |
| [Reality Engine](methodology/09-reality-engine.md) | Closed-loop validation |
| [Planner Logic](methodology/10-planner-logic.md) | Portfolio construction principles |
| [Basis / Cash & Carry](methodology/11-basis-cash-and-carry.md) | Delivery futures basis trading methodology |

## Key Concepts

### APR Tiers

```
Gross APR            Raw annualized funding rate (what exchanges show)
    ↓
Fee-Adjusted APR     After round-trip trading fees (both legs)
    ↓
Decay-Adjusted APR   After deterministic decay adjustment
                     (rates compress as capital crowds in)
    ↓
Model-Adjusted APR   After trust haircut based on empirical forecast error
                     This is the primary FYOS metric
```

### Trust Grading

FYOS grades each market segment by comparing predictions to outcomes:

| Grade | p75 Absolute Error | Interpretation |
|-------|-------------------|----------------|
| A | ≤ 2% | High confidence |
| B | ≤ 5% | Moderate confidence |
| C | ≤ 10% | Low confidence |
| D | > 10% or insufficient data | Unreliable |

### Capacity Buckets

Position sizing recommendations based on liquidity impact:

| Capacity | Description |
|----------|-------------|
| Deep | Large positions viable with minimal impact |
| Standard | Moderate positions viable |
| Thin | Only small positions advisable |
| Unavailable | Insufficient data to assess |

## What This Repository Does NOT Contain

- Production source code
- Exchange API adapters
- Database schemas or migrations
- Authentication or billing logic
- Proprietary calibration constants
- Real-time data pipelines
- Infrastructure configuration

## Related Resources

- [FYOS Platform](https://fyos.pro)
- [Reality Engine](https://fyos.pro/reality) — Live validation metrics
- [Methodology Docs](https://docs.fyos.pro/methodology) — Full documentation
- [Basis / C&C Docs](https://docs.fyos.pro/basis) — Cash & Carry methodology

## Methodology Changelog

### 2026-04-21 — Interval calculation fix

Discovered Binance snapshot cadence (1h) was mistaken for funding interval (8h), inflating APR 8x. Fixed by always trusting exchange metadata over observed snapshot frequency.

### 2026-04 — Survivability deprecated

Survivability model replaced by deterministic decay. Rationale: survivability implied a probability guarantee that the model could not support empirically. Canonical replacement: decay-adjusted return.

### 2026-04 — Basis / Cash & Carry added

Second strategy family added covering delivery futures basis trading. Primary metric: model_adjusted_basis_apr.

## License

This methodology documentation is licensed under [CC BY-NC 4.0](LICENSE.md).

You may share and adapt for non-commercial purposes with attribution. Commercial use requires written permission.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on methodology discussions and corrections.
