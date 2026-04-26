# Glossary

Key terms used throughout FYOS methodology documentation.

---

### Adjusted APR

The final yield estimate after all deductions (fees, survivability, trust haircut). Represents the conservative expected return.

### Capacity

The position size at which an opportunity remains profitable. Beyond capacity, execution costs erode the edge.

### Cold Start

The state when insufficient historical data exists to compute empirical trust grades. Conservative defaults are applied.

### Conservative APR

A floor estimate that's always at least 50% of expected APR, preventing excessive pessimism.

### Degradation Factor

The fraction of gross yield lost due to survivability concerns. `degradation_factor = 1 - survivability_score`.

### Directional Accuracy

The percentage of predictions where the predicted direction (positive/negative funding) matched the realized direction.

### Edge Value

A composite score combining adjusted APR, capacity, and trust quality for ranking opportunities.

### Expected Realized APR

Net APR adjusted for survivability. Represents yield expectation before trust haircut.

### Funding Rate

The periodic payment between long and short perpetual futures positions. Positive = longs pay shorts.

### Gap Penalty

Reduction applied when current funding rate deviates significantly from historical average.

### Gross APR

Raw annualized funding rate without any adjustments. What exchanges typically display.

### Haircut

The percentage deducted from expected APR to account for forecast uncertainty.

### Half-Life

The time (in hours) for a funding rate deviation to decay by 50% toward its long-term mean.

### Mirage

A funding rate opportunity that appears attractive but cannot be profitably captured due to transience or execution friction.

### Mirage Signal

A qualitative indicator (high/medium/low) of mirage risk based on survivability score.

### Net APR

Gross APR minus trading fees. The yield after execution costs but before survivability and trust adjustments.

### p75 Error

The 75th percentile of absolute prediction errors. Used for trust grade thresholds.

### Persistence Score

A measure (0-1) of how likely a funding rate is to persist over a holding period. Derived from half-life.

### Reality Engine

FYOS's closed-loop validation system that tracks predictions against realized outcomes.

### Reality Gap

The difference between current funding rate and historical average. Large gaps suggest mean-reversion pressure.

### Regime

The current funding rate state (positive high, positive low, neutral, negative low, negative high).

### Reliability Grade

A letter grade (A/B/C/D) indicating forecast reliability based on historical prediction error.

### Resilience Bucket

A classification (strong/acceptable/fragile/critical) of portfolio stress test performance.

### Segment Key

The grouping identifier for trust grades, typically combining exchange, asset class, and evaluation horizon.

### Sign Flip

When a funding rate changes direction (positive → negative or vice versa).

### Soft Capacity

The position size where edge begins degrading. A conservative limit for position sizing.

### Survivability

The probability that a funding opportunity will persist long enough to be profitably captured.

### Survivability Score

A numeric value (0-1) combining persistence and reality gap factors.

### Trust Grade

See Reliability Grade.

### Trust Haircut

The percentage deducted from expected APR based on empirical forecast error.

### Trust Quality

A qualitative level (high/medium/low/unavailable) derived from trust grade.

### Validation

The process of comparing predictions to realized outcomes to measure accuracy.

### Worst-Case Return

The expected portfolio return under adverse stress scenarios.
