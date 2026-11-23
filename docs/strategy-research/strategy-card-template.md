# Strategy research card — TEMPLATE

**Hypothesis ID:** ___ · **Author:** ___ · **Created before testing:** ___

## Market mechanism

Who is on the other side? What structural behavior or compensation is hypothesized? State at least one plausible reason the effect could disappear.

## Eligible universe / data

Venue, segment, contract lifecycle, start/end period, timestamp availability, data release ID, excluded instruments and rights.

## Strategy rule

Define a deterministic signal and trade action using only available inputs. State bar-close versus next-open execution and how intraday gaps are handled. Provide a trivial benchmark.

## Cost and risk model

Commissions, taxes, half-spread, expected slippage, impact, margin, liquidity/capacity, drawdown limits, stale-data policies.

## Evaluation plan

Chronological development/validation/holdout, parameter search count, robust inference, controls and no-lookahead checks.

## Falsification criteria

What evidence would convince us the hypothesis is wrong or economically insignificant after costs?

## Reproducibility

Dataset release, code commit, seed, environment lock, model version, parameters, execution simulator version and evaluation date.

## Outcome

**Accept / reject / investigate further**, with performance *and* uncertainty. An attractive historical chart alone is never evidence of future profitability.
