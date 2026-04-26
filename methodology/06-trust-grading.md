# Trust Grading

Trust grading quantifies how reliable FYOS predictions have been for similar opportunities in the past. It provides an empirically-grounded measure of forecast uncertainty.

## The Trust Problem

All yield predictions contain error. The question is: how much error?

Rather than guess, FYOS measures prediction error empirically by comparing past predictions to realized outcomes. This error profile becomes the **trust grade**.

## Reliability Grades

| Grade | p75 Absolute Error | Interpretation |
|-------|-------------------|----------------|
| A | ≤ 2% | High reliability |
| B | ≤ 5% | Moderate reliability |
| C | ≤ 10% | Low reliability |
| D | > 10% or cold start | Unreliable |

### p75 Threshold

We use the 75th percentile (p75) rather than mean or median because:
- Mean is skewed by outliers
- Median ignores the tail
- p75 captures "typical worst case" — what happens when things go somewhat wrong

## Haircut Derivation

The trust grade translates into an **empirical haircut**:

```
haircut_pct = p75_absolute_error
confidence_cap = 100% - haircut_pct
```

**Example**: If p75 error is 8%, haircut is 8%, confidence cap is 92%.

The adjusted APR incorporates this:
```
adjusted_apr = expected_apr × (1 - haircut_pct)
```

## Segment Keys

Trust grades are computed per **segment** — a grouping of similar opportunities:

- Exchange (Binance, Bybit, OKX, etc.)
- Asset class (BTC, ETH, altcoins)
- Evaluation horizon (8h, 24h, 72h)

Different segments have different error profiles. BTC on Binance may be grade A while an altcoin on a smaller exchange may be grade C.

## Cold Start

When insufficient history exists for a segment:
- **Sample count < 30**: Cold start haircut applied (conservative default)
- **Grade assigned: D** regardless of observed error
- **Haircut**: Typically 15-20%

As observations accumulate, the segment transitions from cold start to empirical grading.

## Recalibration

Trust grades are recomputed periodically:

1. New realized outcomes are recorded
2. Prediction error profiles are updated
3. Grades are reassigned based on updated p75

This creates a **closed feedback loop** where predictions that consistently miss are penalized, while accurate predictions earn higher trust.

## Trust Quality Levels

For user display, trust grades map to quality levels:

| Quality | Grades | User Message |
|---------|--------|--------------|
| High | A | "High confidence in this estimate" |
| Medium | B | "Moderate confidence" |
| Low | C, D | "Low confidence, treat with caution" |
| Unavailable | — | "Insufficient data for grading" |

## Limitations

- Past performance doesn't guarantee future accuracy
- Regime changes can invalidate historical error profiles
- Segment definitions are somewhat arbitrary

Trust grading is a heuristic, not a guarantee. It provides honest disclosure of historical forecast performance.
