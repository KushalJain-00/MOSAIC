# Graph atlas — how to read these pictures

## 1. Option payoff is not profit prediction

![Synthetic long call and put expiry payoff](assets/option-payoffs-synthetic.png)

The graph shows terminal net payoff for **illustrative option premiums** and excludes fees, contract multipliers and expiry settlement mechanics. Ask: at which prices does each position break even? Why can't a static payoff describe path-dependent margin/execution risk?

## 2. Risk estimates lag volatility changes

![Synthetic rolling annualized volatility with regime shift](assets/volatility-regime-synthetic.png)

The plot is synthetic. The sudden increase in return dispersion is followed by a lagged rise in rolling volatility. Ask: what happens to a volatility-targeting strategy just before the estimate catches up?

## 3. Bayes and base rates

![Posterior probability vs false positive rate](assets/bayes-base-rates-synthetic.png)

Hypothetical prior 10%, sensitivity 70%, varying false positive rate. Ask: why does improving specificity matter more when the target event is rare?

## 4. End-to-end research causality

```mermaid
flowchart LR
 MARKET[Market observations] --> QUAL[Source, time, identity checks]
 QUAL --> DATA[Versioned dataset]
 DATA --> FEATURES[Past-only features]
 FEATURES --> HYP[Pre-registered hypothesis]
 HYP --> TEST[Costs and OOS validation]
 TEST --> RISK[Independent risk constraints]
 RISK --> REPORT[Audited evidence, not promised profits]
```
