# APR Progression: From Gross to Deployable

The funding rate displayed on an exchange is rarely the return you can actually capture. FYOS models a progression of yield adjustments that transform headline rates into realistic expectations.

## The Four APR Tiers

### 1. Gross APR

The raw annualized funding rate, calculated as:

```
gross_apr = funding_rate × intervals_per_day × 365
```

For an 8-hour funding interval:
```
gross_apr = funding_rate × 3 × 365 = funding_rate × 1095
```

**Problem**: This assumes you can enter and exit positions for free, that the rate persists indefinitely, and that your prediction of the rate is perfectly accurate.

### 2. Fee-Adjusted APR

After accounting for trading costs:

```
fee_adjusted_apr = gross_apr - (entry_fee + exit_fee) × annualization_factor
```

Where fees are expressed as basis points (bps) of position notional.

**Example**: With 5bps entry + 5bps exit on a 24-hour hold:
```
fee_drag_apr = 0.001 × (365 / 1) = 36.5%
```

A 40% gross APR becomes ~3.5% fee-adjusted on a single day.

For longer holds, fee drag decreases:
```
fee_drag_apr = 0.001 × (365 / holding_days)
```

### 3. Decay-Adjusted APR

Adjusts for deterministic rate compression over time:

```
decay_adjusted_apr = fee_adjusted_apr × decay_factor
```

Where `decay_factor` (0 to 1) is derived from:
- **Half-life model**: How quickly rates compress toward equilibrium
- **Reality gap penalty**: Divergence from long-term averages

A market with short half-life (rates compress quickly) or high reality gap (current rate far from historical norm) receives a lower decay factor.

See: [Deterministic Decay](05-deterministic-decay.md)

### 4. Model-Adjusted APR (Trust-Haircut)

Incorporates empirical forecast error:

```
model_adjusted_apr = decay_adjusted_apr × (1 - haircut_pct)
```

The haircut is derived from the p75 absolute prediction error for that market segment. If past predictions for similar opportunities were off by 8% at the 75th percentile, the model-adjusted APR reflects that uncertainty.

**This is the primary FYOS metric.**

## Why This Matters

| Scenario | Gross APR | Fee-Adj | Decay-Adj | Model-Adj |
|----------|-----------|---------|-----------|-----------|
| Stable high-yield | 50% | 45% | 38% | 35% |
| Volatile spike | 120% | 115% | 46% | 38% |
| Low-persistence | 25% | 20% | 8% | 6% |

The first two scenarios have similar model-adjusted yields despite 70%+ difference in gross rates. The "spike" scenario is heavily penalized by decay adjustment.

## Conservative Factor

For some display contexts, FYOS computes a **conservative floor**:

```
conservative_factor = 0.5 + (decay_factor × 0.5)
conservative_apr = decay_adjusted_apr × conservative_factor
```

This provides a floor that's always at least 50% of decay-adjusted APR, avoiding excessive pessimism when decay factors are very low.

## Design Principle

The APR progression exists to prevent **mirage chasing** — pursuing headline rates that evaporate before capture. Each deduction has empirical justification from closed-loop validation.
