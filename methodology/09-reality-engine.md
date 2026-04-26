# Reality Engine

The Reality Engine is FYOS's closed-loop validation system. It tracks predictions against outcomes to measure and improve forecast accuracy.

## Design Philosophy

Most analytics platforms show predictions without accountability. The Reality Engine inverts this:

1. **Record predictions** at decision time
2. **Observe outcomes** after holding period
3. **Compare and measure** prediction error
4. **Update trust grades** based on realized performance

This creates a feedback loop where inaccurate predictions are penalized and accurate predictions build trust.

## What Gets Validated

### 1. Funding Rate Predictions

- **Predicted**: Expected funding over holding period
- **Realized**: Actual funding received
- **Error**: |predicted - realized|

### 2. Decay Predictions

- **Predicted**: Decay factor (0-1)
- **Realized**: Actual rate retention
- **Metric**: Decay accuracy rate

### 3. Half-Life Predictions

- **Predicted**: Expected half-life (hours)
- **Realized**: Observed decay rate
- **Error**: Absolute half-life error (hours)

### 4. Mirage Predictions

- **Predicted**: Mirage signal (high/medium/low)
- **Realized**: Did rate persist or collapse?
- **Metrics**: Under-penalty rate, over-penalty rate

### 5. Planner Predictions

- **Predicted**: Portfolio expected return
- **Realized**: Actual portfolio return
- **Metric**: Return error, beat rate

## Validation Pipeline

```
[Opportunity Snapshot] → [Prediction Record] → [Holding Period]
                                                      ↓
                                          [Realized Outcome]
                                                      ↓
                                          [Error Calculation]
                                                      ↓
                                          [Profile Update]
                                                      ↓
                                          [Grade Recomputation]
```

The pipeline runs continuously, processing new outcomes as they mature.

## Public Metrics

The Reality Engine exposes aggregate metrics publicly:

| Metric | Description |
|--------|-------------|
| Median Error | Typical prediction error |
| p75 Error | 75th percentile error |
| p90 Error | 90th percentile error |
| Directional Accuracy | % where predicted direction matched |
| Sign Flip Rate | % where direction reversed |
| Decay Accuracy | % where decay predictions were accurate |

These metrics are displayed on the public Reality page with full transparency.

## Exchange Breakdown

Error profiles are computed per exchange:

```
EXCHANGE    Median Error    Samples    Directional Accuracy
BINANCE     2.1%           1,247      87%
BYBIT       2.8%           892        84%
OKX         3.4%           654        82%
```

Different exchanges have different forecast reliability.

## Calibration Phase

When sample sizes are low (<30 observations), the system is in **calibration phase**:

- Metrics are displayed with appropriate caveats
- Trust grades use cold-start haircuts
- Validation status is clearly communicated

As samples accumulate, the system transitions to empirical grading.

## Continuous Improvement

The Reality Engine enables methodology refinement:

1. **Identify systematic errors**: Consistent over-prediction → adjust model
2. **Detect regime changes**: Sudden accuracy drop → investigate
3. **Validate new features**: A/B test against baseline

This closes the loop between model development and real-world performance.

## Limitations

- Validation requires time (holding periods must elapse)
- Small samples produce noisy estimates
- Structural breaks invalidate historical patterns
- Not all predictions are equally important

The Reality Engine is a tool for honest self-assessment, not a guarantee of future accuracy.
