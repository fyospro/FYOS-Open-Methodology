# Survivability

Survivability quantifies the probability that a funding rate opportunity will persist long enough to be profitably captured.

## The Survivability Score

```
survivability_score = persistence_score × gap_penalty
```

Where:
- `persistence_score`: From half-life model (see [Funding Persistence](02-funding-persistence.md))
- `gap_penalty`: From reality gap analysis (see [Reality Gap](04-reality-gap.md))

The score ranges from 0 (will not survive) to 1 (very likely to survive).

## Interpretation

| Score | Interpretation | Typical Scenario |
|-------|----------------|------------------|
| 0.80+ | High survivability | Stable regime, rate near historical norm |
| 0.50-0.79 | Moderate survivability | Some regime uncertainty |
| 0.25-0.49 | Low survivability | Elevated gap or short half-life |
| <0.25 | Very low survivability | Extreme deviation, likely mirage |

## Impact on Yield

The survivability score directly scales expected returns:

```
expected_realized_apr = net_apr × survivability_score
```

**Example**: A 40% net APR with 0.60 survivability becomes 24% expected.

This adjustment is applied before trust haircuts, so the final adjusted APR may be lower still.

## Degradation Factor

For transparency, FYOS exposes the **degradation factor** — how much yield is being deducted due to survivability concerns:

```
degradation_factor = 1 - survivability_score
```

A degradation factor of 0.40 means 40% of net yield is being discounted.

## Survivability Signals

The mirage signal derives from survivability:

| Survivability | Mirage Signal |
|--------------|---------------|
| ≥ 0.75 | Low |
| ≥ 0.40 | Medium |
| < 0.40 | High |

Users see both the numeric score and the qualitative signal.

## Validation

The Reality Engine validates survivability predictions:

1. **Prediction**: At opportunity time, record survivability score
2. **Observation**: Track actual funding over holding period
3. **Comparison**: Did the rate survive as predicted?

**Survived**: Realized funding was ≥ 50% of predicted
**Did not survive**: Realized funding was < 50% of predicted

This binary outcome allows computing the **survivability rate** — the fraction of opportunities that survived as predicted.

## Limitations

- Survivability is a statistical expectation, not a guarantee
- Extreme market events can invalidate any prediction
- Correlations between markets are not captured (systemic risk)

FYOS treats survivability as "best current estimate" and always pairs it with trust grading to communicate uncertainty.
