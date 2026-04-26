# Capacity Modeling

Capacity modeling estimates how much capital can be deployed into a funding opportunity before execution costs erode the edge.

## The Scalability Myth

A 40% APR on $1,000 is different from 40% APR on $10,000,000:

- Small positions: Minimal market impact, full edge capture
- Large positions: Slippage on entry/exit, funding rate compression

Capacity modeling answers: "At what position size does this opportunity become unprofitable?"

## Capacity Components

### 1. Liquidity-Based Capacity

Derived from order book depth and trading volume:

```
liquidity_capacity = reference_liquidity × impact_tolerance
```

Where `impact_tolerance` is the acceptable slippage (e.g., 0.1% of edge).

### 2. Edge-Based Capacity

The point where execution costs consume the edge:

```
edge_capacity = net_apr / (slippage_per_dollar × annualization)
```

If slippage grows linearly with size, there's a maximum profitable position.

### 3. Crowding Adjustment

If aggregate interest exceeds capacity:

```
crowding_penalty = min(1, capacity / estimated_demand)
effective_capacity = capacity × crowding_penalty
```

Popular opportunities may have reduced effective capacity.

## Capacity Buckets

For user consumption, capacity maps to buckets:

| Bucket | Position Guidance |
|--------|-------------------|
| Deep | $500K+ positions viable |
| Standard | $50K-500K positions viable |
| Thin | <$50K positions advised |
| Unavailable | Cannot assess |

Buckets are relative to asset liquidity. "Thin" on BTC might still be $200K.

## Soft vs. Hard Capacity

- **Soft capacity**: Position size where edge begins degrading
- **Hard capacity**: Position size where edge reaches zero

FYOS displays soft capacity — the conservative limit where the opportunity remains attractive.

## Reference Capacity

For validation purposes, FYOS tracks **reference capacity** — the median soft capacity across the opportunity set. This provides:

- Baseline for "normal" position sizing
- Comparison metric for unusual capacity events
- Input for planner allocation constraints

## Capacity in Practice

Capacity affects:

1. **Screener display**: Opportunities flagged if capacity is thin
2. **Planner allocation**: Positions capped at soft capacity
3. **Trust warnings**: Low capacity triggers "thin liquidity" warning

## Limitations

- Capacity is estimated, not measured
- Large coordinated flows can exhaust capacity suddenly
- Order book snapshots may not reflect executable depth
- Hidden liquidity (icebergs, RFQ) is not captured

Capacity should be treated as order-of-magnitude guidance, not precise limits.

## Validation Status

Capacity validation is in **reference-only** phase:

- Reference capacity is tracked
- Realized capacity impact is not yet measured
- Future work: track slippage vs. predicted capacity

The Reality Engine will eventually validate capacity predictions against execution reports.
