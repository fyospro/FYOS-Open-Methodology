# Basis / Cash & Carry

## Overview

Cash & Carry (C&C) is the second strategy family supported by FYOS. It involves simultaneously holding spot and shorting delivery futures to capture the basis premium at contract expiry.

## Key Difference from Funding Arbitrage

| Aspect | Funding Arbitrage | Cash & Carry |
|--------|------------------|--------------|
| Instrument | Perpetual futures | Delivery futures |
| Duration | Indefinite | Fixed (to expiry) |
| Settlement | Periodic (8h) | At expiry |
| Risk | Rate compression | Basis convergence |

## Core Metrics

### gross_basis_apr

Raw annualized basis before any execution adjustments.

```
gross_basis_apr = (future_price - spot_price) / spot_price
                  × (365 / days_to_expiry) × 100
```

**This is a diagnostic metric only. Do not use as deployable return.**

### model_adjusted_basis_apr

The primary FYOS basis metric. After all reality adjustments:

```
gross_basis_apr
    → fee_adjusted_basis_apr    (after trading fees, both legs)
    → slippage_adjusted_apr     (after spread crossing)
    → model_adjusted_basis_apr  (after trust haircut)
```

This mirrors the funding arbitrage APR progression but with basis-specific adjustments.

## Dual-Leg Capacity

Unlike perpetual funding, C&C requires simultaneous execution on both spot and futures legs. Capacity is limited by the weaker leg:

```
dual_leg_capacity = min(spot_capacity, futures_capacity)
```

Both legs must execute successfully for the trade to work.

## Trust Layer for Basis

Additional trust considerations for C&C:

| Factor | Haircut | Rationale |
|--------|---------|-----------|
| Inverse contracts | +10% | Settlement in base asset adds risk |
| Low confidence mapping | +15% | Sparse historical data |
| Short expiry (<14d) | +5% | Less time to recover from adverse moves |

## Expiry Buckets

C&C opportunities are categorized by time to expiry:

| Bucket | Days to Expiry | Characteristics |
|--------|---------------|-----------------|
| Short | <14 days | Higher annualized APR, less flexibility |
| Medium | 14-45 days | Balanced risk/return |
| Long | >45 days | Lower annualized APR, more flexibility |

## Current Market Context

Basis premiums are thin in current market conditions:

- Model-adjusted APR at standard fees (5bps): typically 0.1-0.5%
- At VIP/maker fees (2bps): opportunities improve to 0.5-1.0%
- Occasional spikes during high volatility periods

## Fee Tier Impact

Fee tier selection critically affects C&C viability:

| Fee Tier | Round-trip Cost | Impact on 0.25% basis |
|----------|----------------|----------------------|
| Standard (5 bps) | ~20 bps | Often negative after fees |
| VIP/Maker (2 bps) | ~8 bps | Typically positive |

At standard retail fees, most basis opportunities are not profitable after execution costs.

## Comparison to Funding

| Metric | Funding Arbitrage | Cash & Carry |
|--------|------------------|--------------|
| Typical APR | 5-30% | 0.5-5% |
| Duration | 8h-72h typical | Weeks to months |
| Rate risk | Compression/reversal | Convergence guaranteed |
| Complexity | Single instrument | Dual-leg execution |

C&C offers more certainty (basis must converge at expiry) but lower yields.

## Simulator Support

FYOS provides a basis simulator for:

- Hold-to-expiry scenarios
- Early exit scenarios (what if basis widens?)
- Fee sensitivity analysis

See: [FYOS C&C Screener](https://app.fyos.pro/screener?mode=basis)

## Related

- [APR Progression](01-apr-progression.md)
- [Capacity Modeling](07-capacity-modeling.md)
- [Trust Grading](06-trust-grading.md)
- [Planner Logic](10-planner-logic.md)
