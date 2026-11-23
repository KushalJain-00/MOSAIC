# Strategy validation checklist

## Financial correctness

- [ ] Security/contract universe reflects listings known at the simulated date.
- [ ] Corporate actions and derivative adjustments handled with dated source evidence.
- [ ] Expired contracts preserved; futures rolls explicitly simulated.
- [ ] Units, contract multiplier and lot sizes correct at each timestamp.
- [ ] No unrealistic fill assumptions based solely on midpoint or candle highs/lows.

## Statistical correctness

- [ ] Inputs known at decision timestamp, not just labelled earlier.
- [ ] Training/validation/holdout chronological and documented.
- [ ] Parameter search and multiple tests recorded.
- [ ] Uncertainty and regime sensitivity reported.
- [ ] Performance survives feasible costs, delays and missing data.
- [ ] One meaningful negative control included.

## Reproducibility

- [ ] Exact source and dataset release hashes.
- [ ] Code commit, dependencies, parameters and seed.
- [ ] Execution simulator version, cost assumptions and calendar.
- [ ] Limitations and unverified observations disclosed.

## Safety

- [ ] Not used for automatic live trading approval.
- [ ] Independent risk authority and legal/venue approvals required before any eventual live integration.
