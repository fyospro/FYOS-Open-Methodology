# Reality Gap

The reality gap measures how far current funding conditions have deviated from historical norms. Large gaps indicate potential mean-reversion pressure.

## Definition

```
reality_gap = current_rate - reference_rate
```

Where:
- `current_rate`: Most recent funding rate (or short-term average)
- `reference_rate`: Historical average funding rate for this market

## Why It Matters

Funding rates are not independent — they're driven by underlying market positioning. When rates deviate significantly from historical levels:

1. **Arbitrageurs notice**: Large deviations attract capital
2. **Positions adjust**: Funding pressure reduces as positions rebalance
3. **Mean reversion occurs**: Rates return toward historical levels

A 60% gross APR when historical average is 15% has a reality gap of 45%. This gap represents "borrowed" yield that is likely to be repaid through reversion.

## Reference Rate Selection

The reference rate can be computed over different windows:

| Window | Use Case |
|--------|----------|
| 30-day | Short-term regime awareness |
| 90-day | Medium-term structural level |
| 365-day | Long-term equilibrium estimate |

FYOS uses context-appropriate windows, typically 90-180 days for mature markets.

## Gap Penalty

The reality gap feeds into survivability calculation:

```
normalized_gap = min(|reality_gap| / 0.05, 1)
gap_penalty = 1 - normalized_gap
```

Where `0.05` (5% annualized) represents the threshold for full penalty.

### Example

| Current Rate | Reference | Gap | Penalty |
|-------------|-----------|-----|---------|
| 18% | 15% | 3% | 0.40 |
| 35% | 15% | 20% | 0.00 |
| 16% | 15% | 1% | 0.80 |
| 15% | 15% | 0% | 1.00 |

The 35% rate receives zero credit for persistence because the gap exceeds the full-penalty threshold.

## Directional Gap

The gap can be positive or negative:

- **Positive gap**: Current funding higher than historical (longs paying shorts)
- **Negative gap**: Current funding lower than historical (shorts paying longs)

Both directions trigger mean-reversion expectations, though directional changes (sign flips) are particularly penalized.

## Sign Flip Risk

A **sign flip** occurs when funding changes direction (positive → negative or vice versa). Sign flips are especially damaging because:

1. The initial position loses the expected funding
2. The trade may go from "paid to hold" to "paying to hold"
3. Exit timing becomes critical

FYOS tracks sign flip rates per market as a trust metric.

## Limitations

- Reference rates assume stationary long-term behavior
- Structural changes (new liquidity, regulatory shifts) invalidate references
- Short market histories produce unreliable references

When reference data is insufficient, FYOS indicates unavailable status rather than using unreliable estimates.
