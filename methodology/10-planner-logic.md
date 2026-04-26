# Planner Logic

The Capital Deployment Planner constructs portfolios from individual funding opportunities, applying diversification constraints and stress testing.

## Design Goals

1. **Maximize risk-adjusted return**: Not just highest yield
2. **Enforce diversification**: No single position dominates
3. **Respect capacity**: Don't exceed soft capacity limits
4. **Validate resilience**: Stress test before recommending

## Portfolio Construction

### Step 1: Candidate Selection

Filter opportunities by:
- Minimum edge value score
- Trust quality threshold (optionally exclude low confidence)
- Positive adjusted APR

### Step 2: Position Sizing

For each candidate, allocate capital respecting:

```
allocation ≤ min(
    max_allocation_per_position,
    soft_capacity_usd,
    total_capital × max_position_pct
)
```

### Step 3: Concentration Limits

Enforce diversification:

```
exchange_allocation ≤ total_capital × max_exchange_concentration_pct
```

No single exchange should dominate the portfolio.

### Step 4: Global Optimization

Allocate across candidates to maximize:

```
portfolio_edge_value = Σ (allocation_i × adjusted_apr_i × quality_multiplier_i)
```

Subject to:
- Total allocation ≤ total_capital
- Per-position limits
- Per-exchange limits

## Marginal Allocation

The planner uses **marginal allocation** — adding positions incrementally until constraints bind:

1. Rank candidates by edge contribution per dollar
2. Allocate to top candidate until its constraint binds
3. Move to next candidate
4. Repeat until capital exhausted or no candidates remain

This produces efficient capital deployment without full combinatorial optimization.

## Stress Testing

After initial allocation, the portfolio undergoes stress scenarios:

| Scenario | Description |
|----------|-------------|
| Rate collapse | All rates go to zero |
| Adverse funding | Rates flip to opposite direction |
| Partial mean reversion | Rates move 50% toward historical average |
| Fee spike | Trading costs double |

Each scenario produces a **worst-case return** estimate.

## Resilience Buckets

Based on stress results, portfolios receive a resilience classification:

| Bucket | Criteria |
|--------|----------|
| Strong | Positive return in all scenarios |
| Acceptable | Positive return in most scenarios |
| Fragile | Negative return in stress scenarios |
| Critical | Severe losses in stress scenarios |

## Deployment Decision

The planner produces a deployment recommendation:

| Decision | Meaning |
|----------|---------|
| Strong Candidate | Conservative view supports deployment |
| Needs Review | Mixed resilience, human judgment required |
| Do Not Deploy | Conservative view does not support deployment |

### Decision Logic

```python
if resilience == "strong" or (resilience == "acceptable" and retention ≥ 60%):
    return "strong_candidate"
if resilience == "acceptable" or (resilience == "fragile" and worst_case > 0):
    return "needs_review"
return "do_not_deploy"
```

## Output Components

The planner returns:

1. **Allocations**: List of positions with sizes
2. **Summary**: Expected return, risk metrics
3. **Warnings**: Concentration issues, assumption flags
4. **Diagnostics**: Allocation efficiency, unused capacity
5. **Stress Results**: Per-scenario outcomes
6. **Deployment Decision**: Recommendation with reasoning

## Limitations

- Expected returns are projections, not guarantees
- Stress scenarios are deterministic, not probabilistic
- Correlations between positions are not fully modeled
- Execution timing is not considered

The planner provides a **bounded recommendation** — a conservative starting point, not a trading signal.

## Validation

Planner predictions are validated by the Reality Engine:

- Track predicted vs. realized portfolio returns
- Measure beat rate (did realized exceed expected?)
- Update trust grades for planner outputs

This validates whether planner conservatism is appropriately calibrated.
