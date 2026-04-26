# Edge Value

Edge Value is a composite score that ranks opportunities by their expected risk-adjusted return, incorporating yield, decay adjustment, trust, and capacity.

## The Ranking Problem

Raw APR is insufficient for ranking:
- 80% APR with 0.2 decay factor < 30% APR with 0.9 decay factor
- 40% APR on $10K capacity < 25% APR on $1M capacity
- High yield with grade D trust < moderate yield with grade A

Edge Value synthesizes these factors into a single comparable metric.

## Calculation Framework

```
edge_value = model_adjusted_apr × capacity_factor × quality_bonus
```

### Component 1: Model-Adjusted APR

The trust-haircut-adjusted yield (see [APR Progression](01-apr-progression.md)):

```
model_adjusted_apr = fee_adjusted_apr × decay_factor × (1 - haircut)
```

### Component 2: Capacity Factor

Scales edge by deployable size:

```
capacity_factor = log(1 + soft_capacity_usd / reference_capacity)
```

Logarithmic scaling prevents large-capacity opportunities from dominating.

### Component 3: Quality Bonus

Trust grades receive multipliers:

| Grade | Multiplier |
|-------|------------|
| A | 1.20 |
| B | 1.00 |
| C | 0.80 |
| D | 0.60 |

High-trust opportunities are prioritized; low-trust opportunities are discounted.

## Edge Value Tiers

For display, edge value maps to tiers:

| Tier | Edge Value | Interpretation |
|------|-----------|----------------|
| Exceptional | ≥ 0.80 | Rare, high-confidence opportunity |
| Strong | 0.50-0.79 | Good opportunity |
| Moderate | 0.25-0.49 | Acceptable |
| Weak | < 0.25 | Low expected value |

## Crowding Consideration

Edge value incorporates crowding awareness:

- If many participants target the same opportunity, execution degrades
- Crowding penalties reduce effective edge value
- Diversification across opportunities is implicitly encouraged

## Use Cases

### 1. Screener Ranking

Opportunities sorted by edge value place the "best" opportunities (risk-adjusted, capacity-aware, trust-qualified) at the top.

### 2. Planner Selection

The capital deployment planner uses edge value to select which opportunities to include in a portfolio.

### 3. Threshold Filtering

Users can set minimum edge value requirements to filter out weak opportunities.

## Limitations

- Edge value is model-dependent; different weightings produce different rankings
- Cross-asset comparisons may have structural biases
- Single scores lose information present in components

Edge value is a convenience metric for ranking, not a replacement for examining individual components.

## Transparency

FYOS exposes all edge value components:
- Model-adjusted APR
- Decay factor
- Trust grade
- Capacity bucket

Users can verify the ranking logic and override with their own judgment.
