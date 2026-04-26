# Deterministic Decay

## Overview

FYOS models funding rate decay deterministically rather than probabilistically. Rates compress predictably as capital enters crowded trades.

## What This Replaces

Earlier versions of FYOS used a "survivability" model that estimated the probability of edge persisting. This has been replaced by deterministic decay modeling which is more transparent and empirically grounded.

**Survivability as a concept is deprecated in FYOS.** The canonical replacement is decay-adjusted return.

## Core Concept

Funding rates are not static. A rate observed at t=0 will typically compress as:

1. More capital enters the trade
2. Exchange mechanisms normalize the rate
3. Opposing positions accumulate

This compression is largely predictable — not random — hence "deterministic decay."

## Half-Life Model

FYOS estimates the half-life of funding rate elevation:

```
rate(t) = rate(0) × exp(-ln(2) × t / half_life)
```

Where:
- `rate(0)`: Initial observed rate
- `t`: Time elapsed (hours)
- `half_life`: Estimated decay half-life (hours)

**Observed half-lives:**
- Median: 8-11 hours for most instruments
- High-rate environments compress faster
- Extreme spikes may have half-lives under 4 hours

## Decay Factor

The decay factor quantifies expected rate retention over a holding period:

```
decay_factor = exp(-ln(2) × holding_hours / half_life_hours)
```

**Example:** For a 24-hour hold with 36-hour half-life:
```
decay_factor = exp(-0.693 × 24 / 36) = 0.63
```

Approximately 63% of the initial rate is expected to remain.

## Decay-Adjusted Return

The canonical FYOS metric for persistence-adjusted return:

```
decay_adjusted_apr = fee_adjusted_apr × decay_factor
```

This replaces the deprecated "expected realized APR" terminology.

## Why Not Probabilistic?

The previous "survivability" framing implied:
- A probability that edge "survives" (binary outcome)
- Statistical guarantees about persistence

In practice:
- Rates don't binary survive/fail — they compress continuously
- The model cannot provide probability guarantees
- Deterministic framing is more honest about what we're measuring

## Caveats

Half-life estimation is the weakest validated layer in FYOS:
- Empirical error: ~11-12 hours median absolute error
- Structural breaks invalidate historical estimates
- New listings have unreliable half-life data

**Treat decay estimates as directional signals, not precise forecasts.**

See: [Reality Engine](https://fyos.pro/reality) for current half-life validation metrics.

## Implementation Notes

The decay model feeds into:
- APR tier progression (Gross → Fee-Adjusted → Decay-Adjusted → Model-Adjusted)
- Mirage signal calculation
- Planner position sizing

## Related

- [APR Progression](01-apr-progression.md)
- [Funding Persistence](02-funding-persistence.md)
- [Trust Grading](06-trust-grading.md)
- [Reality Engine](09-reality-engine.md)
