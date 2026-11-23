# Option payoffs, moneyness and contractual risks

> **Track:** `indian-markets` · **Lesson:** 3 · **Target:** independent reasoning (4/5)  
> **Prerequisite:** read the track index and complete prior diagnostic questions.  
> **Outcome:** Compute terminal option payoffs and separate payoff from realized P&L.

## Socratic opening — answer before reading

1. Why does a call finishing in-the-money not always mean the buyer made a profit?
2. What is the maximum premium-only loss on a fully paid long call?
3. What market risks make a short uncovered option different from a long option?

**Tutor protocol:** ask one question at a time in a live session. The tutor should not reveal the worked solution until the learner has attempted the problem and been probed on the reasoning. These questions are a study bank, not evidence of mastery.

## Core reasoning

A call provides the right (subject to the contract's exercise style) to buy or cash-settle relative to strike; a put the opposite exposure. The option premium, lot size, brokerage, taxes and expiry settlement rules determine realized P&L. Never confuse payoff per unit with exchange trade quantity or contract multiplier.

## Mathematical or engineering model

Call intrinsic payoff at expiry: $$C_T=\max(S_T-K,0).$$ Put payoff: $$P_T=\max(K-S_T,0).$$ Long call net payoff per unit after premium $c_0$ is $\max(S_T-K,0)-c_0$ before additional costs. Monetary P&L multiplies by the applicable units/contract specification.

## Worked example (read after attempting)

Illustrative call: strike ₹100, premium ₹8, expiry underlying ₹115 ⇒ gross intrinsic ₹15, net ₹7 per unit before other costs. If expiry is ₹95, intrinsic zero and long option premium loss ₹8 per unit.

## Visual model

```mermaid
flowchart LR
 INPUT[Strike, expiry, right, premium] --> STATE[Expiry underlying price]
 STATE --> PAY[Intrinsic payoff]
 PAY --> COST[Subtract premium and fees]
 COST --> MULT[Apply contract quantity]
```

## Failure modes and misconceptions

**Danger:** the short option payoff may be unbounded (for a short call in an unconstrained model); margin and exchange rules are additional constraints not captured by simple payoff graphs.

## Build or paper exercise

Draw payoff curves for long call, long put and a vertical spread. Keep strikes and premiums synthetic. Account for lot size and costs in a second view.

## Verification and cross-connections

Verify dimensional units, edge cases, reproducibility and whether each input was actually observable at decision time.

## Evidence and references

[CFA Institute pricing and valuation of options](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/pricing-valuation-options)

## Mastery gate

To claim **4/5**, solve two independent variants, including one without hints after a delay. To claim **5/5**, explain a counterexample, show an edge case and teach the model clearly. Record evidence in `docs/learning-material/MASTERY-TRACKER.md`.
