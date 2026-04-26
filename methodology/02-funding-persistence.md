# Funding Persistence

Funding rates are not random walks. They exhibit temporal structure — periods of sustained positive or negative funding, punctuated by regime shifts. Persistence measurement captures this structure.

## Why Persistence Matters

A 50% annualized rate that persists for 30 days has different value than one that flips sign within 6 hours:

- **High persistence**: Enter trade, collect multiple intervals, exit
- **Low persistence**: Enter trade, rate reverses, exit at loss or breakeven

Trading costs are paid upfront; funding is collected over time. If the rate doesn't persist long enough to cover fees, the opportunity is illusory.

## Measuring Persistence

FYOS uses a **half-life model** to characterize how funding rates decay toward their long-term mean.

### Half-Life Concept

If rates have a half-life of 12 hours, then:
- After 12 hours, the current deviation from mean is expected to be 50% of initial
- After 24 hours, 25%
- After 48 hours, 6.25%

### Persistence Score

The persistence score (0 to 1) reflects the fraction of initial edge expected to survive over a holding horizon:

```
persistence_score = exp(-ln(2) × holding_hours / half_life_hours)
```

**Example**: For a 24-hour hold with 36-hour half-life:
```
persistence_score = exp(-0.693 × 24 / 36) = 0.63
```

Approximately 63% of the initial edge is expected to remain.

## Half-Life Estimation

Half-life is estimated from historical funding rate series using exponential decay fitting:

1. Compute autocorrelation of funding rates at various lags
2. Fit exponential decay curve
3. Extract half-life parameter

Markets with:
- **Long half-life** (>48h): Stable regimes, mean-reversion is slow
- **Short half-life** (<12h): Volatile, rates revert quickly
- **Unavailable**: Insufficient history or non-stationary behavior

## Regime Detection

Beyond half-life, FYOS detects the current **funding regime**:

| Regime | Description |
|--------|-------------|
| `positive_high` | Funding > 0, elevated |
| `positive_low` | Funding > 0, moderate |
| `neutral` | Near zero |
| `negative_low` | Funding < 0, moderate |
| `negative_high` | Funding < 0, elevated |

Regime labels help contextualize whether current rates are normal or extreme.

## Limitations

- Half-life assumes stationary dynamics; structural breaks invalidate estimates
- Short histories (new listings) produce unreliable estimates
- Extreme events (liquidation cascades) create outliers

When half-life is unavailable or unreliable, FYOS falls back to conservative defaults (short assumed half-life, lower persistence score).

## Validation

The Reality Engine tracks whether predicted half-lives match observed mean-reversion speed:

- **Observed**: Rate actually decayed at predicted rate
- **Not observed**: Rate persisted longer or shorter than predicted

This validation informs trust grades for persistence metrics.
