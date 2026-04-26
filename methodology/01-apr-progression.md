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

### 2. Net APR

After accounting for trading costs:

```
net_apr = gross_apr - (entry_fee + exit_fee) × annualization_factor
```

Where fees are expressed as basis points (bps) of position notional.

**Example**: With 5bps entry + 5bps exit on a 24-hour hold:
```
fee_drag_apr = 0.001 × (365 / 1) = 36.5%
```

A 40% gross APR becomes ~3.5% net on a single day.

For longer holds, fee drag decreases:
```
fee_drag_apr = 0.001 × (365 / holding_days)
```

### 3. Expected Realized APR

Adjusts for the probability that the rate will persist:

```
expected_apr = net_apr × survivability_score
```

Where `survivability_score` (0 to 1) combines:
- **Persistence score**: Historical autocorrelation of rates
- **Reality gap penalty**: Divergence from long-term averages

A market with low persistence (rates flip frequently) or high reality gap (current rate far from historical norm) receives a lower survivability score.

### 4. Adjusted APR (Trust-Haircut)

Incorporates empirical forecast error:

```
adjusted_apr = expected_apr × (1 - haircut_pct)
```

The haircut is derived from the p75 absolute prediction error for that market segment. If past predictions for similar opportunities were off by 8% at the 75th percentile, the adjusted APR reflects that uncertainty.

## Why This Matters

| Scenario | Gross APR | Net APR | Expected APR | Adjusted APR |
|----------|-----------|---------|--------------|--------------|
| Stable high-yield | 50% | 45% | 38% | 35% |
| Volatile spike | 120% | 115% | 46% | 38% |
| Low-persistence | 25% | 20% | 8% | 6% |

The first two scenarios have similar adjusted yields despite 70%+ difference in gross rates. The "spike" scenario is heavily penalized by survivability.

## Conservative Factor

For display purposes, FYOS also computes a **conservative APR**:

```
conservative_factor = 0.5 + (survivability_score × 0.5)
conservative_apr = expected_apr × conservative_factor
```

This provides a floor that's always at least 50% of expected APR, avoiding excessive pessimism when survivability is very low.

## Design Principle

The APR progression exists to prevent **mirage chasing** — pursuing headline rates that evaporate before capture. Each deduction has empirical justification from closed-loop validation.
