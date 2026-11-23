# Probability, expected value and adverse selection

> **Track:** `quant-foundations` · **Lesson:** 1 · **Target:** independent reasoning (4/5)  
> **Prerequisite:** read the track index and complete prior diagnostic questions.  
> **Outcome:** Calculate and interpret an expected payoff under differing probabilities and costs.

## Socratic opening — answer before reading

1. Why is a positive average profit insufficient to deploy a strategy?
2. Can a quoted positive spread yield negative realized expectation?
3. What probability would make a ₹120 win / ₹80 loss exactly break even before costs?

**Tutor protocol:** ask one question at a time in a live session. The tutor should not reveal the worked solution until the learner has attempted the problem and been probed on the reasoning. These questions are a study bank, not evidence of mastery.

## Core reasoning

A probability model enumerates mutually exclusive outcomes and assigns weights summing to 1. Expected value is a **weighted average**, not the outcome you expect on any one trial. Distinguish expectation from variance and from utility under capital constraints. An apparently profitable quoted spread can disappear when counterparties preferentially trade against a stale quote. Do not assume independent bets merely because outcomes look similar.

## Mathematical or engineering model

For payoff random variable $X$ with outcomes $x_i$, $$E[X]=\sum_i p_i x_i,\qquad Var(X)=E[(X-E[X])^2].$$ For trade profit after costs $P=X-C$, if costs are deterministic, $E[P]=E[X]-C$. A quoting edge must be assessed **conditional on a fill**, not only unconditionally.

## Worked example (read after attempting)

You win ₹120 with probability 0.45 and lose ₹80 otherwise. Gross expectation = 0.45×120 − 0.55×80 = **₹10 per trade**. If the round-trip expected fee/slippage is ₹14, net expectation becomes **−₹4**. Now suppose losing trades execute more often than winning ones: the conditional probabilities change further; you must estimate them from fills, not your forecast alone.

## Visual model

```mermaid
flowchart LR
 A[Enumerate outcomes] --> B[Assign probabilities]
 B --> C[Calculate gross expectation]
 C --> D[Condition on actual execution]
 D --> E[Subtract fees and slippage]
 E --> F[Evaluate risk and uncertainty]
```

## Failure modes and misconceptions

**Error:** treating $E[P]>0$ as evidence of stable risk-adjusted profitability. It ignores drawdowns, execution-conditioned outcomes, parameter uncertainty and finite sample error.

## Build or paper exercise

Paper: derive the breakeven win probability when each transaction costs ₹14. Then write a Monte Carlo experiment over 10,000 trades with fixed seed, and show why sample mean can be negative even for a genuinely positive expectancy.

## Verification and cross-connections

Bridge to strategy research: a backtest estimates execution-conditioned profit, not merely signal correctness.

## Evidence and references

[Jane Street Probability & Markets Guide](https://www.janestreet.com/probability-markets/) · [Jane Street puzzles](https://www.janestreet.com/puzzles/)

## Mastery gate

To claim **4/5**, solve two independent variants, including one without hints after a delay. To claim **5/5**, explain a counterexample, show an edge case and teach the model clearly. Record evidence in `docs/learning-material/MASTERY-TRACKER.md`.
