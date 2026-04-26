# Mirage Detection

A **mirage** is a funding rate that appears attractive but cannot be captured in practice. Mirage detection identifies opportunities where the headline yield is unlikely to translate into realized returns.

## What Creates Mirages

### 1. Transient Spikes

Funding rates can spike during:
- Liquidation cascades
- News events
- Exchange-specific imbalances

These spikes often revert within 1-3 intervals, faster than positions can be established and unwound profitably.

### 2. Crowded Trades

When many participants pile into the same trade:
- Execution slippage increases
- The act of entering compresses the opportunity
- Exit liquidity deteriorates

A 100% gross APR shared by $500M notional is not the same as one with $10M notional.

### 3. Structural Mispricings

Some rates reflect:
- Exchange-specific funding formula quirks
- Temporary oracle divergences
- Settlement timing effects

These can look like opportunities but often resolve before capture.

## Mirage Signal

FYOS produces a **mirage signal** (high/medium/low) indicating the likelihood that an opportunity is a mirage.

### Calculation

```
gap_penalty = 1 - min(|reality_gap| / 0.05, 1)
decay_factor = persistence_score × gap_penalty
```

Where:
- `reality_gap` = current rate - historical average rate
- `0.05` = threshold for full penalty (5% gap)

The decay factor is then mapped to mirage signal:

| Decay Factor | Mirage Signal |
|--------------|---------------|
| ≥ 0.75 | Low |
| ≥ 0.40 | Medium |
| < 0.40 | High |

### Interpretation

- **Low mirage risk**: Rate is near historical norm, regime is stable
- **Medium mirage risk**: Some deviation from norm, proceed with caution
- **High mirage risk**: Rate is extreme outlier, high probability of reversion

## Mirage vs. Genuine Opportunity

The difference between a mirage and a genuine opportunity:

| Factor | Mirage | Genuine |
|--------|--------|---------|
| Rate magnitude | Extreme (>80% APR) | Moderate (10-40% APR) |
| Persistence | Hours | Days to weeks |
| Reality gap | Large (>3%) | Small (<1%) |
| Regime | Unstable | Consistent |

## Conservative Handling

When mirage risk is high, FYOS:
1. Applies aggressive decay penalty
2. Displays prominent warnings
3. Excludes from planner recommendations (if trust threshold enabled)

Users can still view high-mirage opportunities but receive clear indication that the displayed yield is unlikely to be realized.

## Validation

The Reality Engine tracks mirage predictions:

- **Underpenalty**: Predicted stable, actually collapsed
- **Overpenalty**: Predicted mirage, actually persisted

This feedback refines mirage detection thresholds over time.
