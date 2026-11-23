# MOSAIC Quant Formula Reference

> Compact reference, **not** a proof of proficiency. Read the matching learning lesson for assumptions, examples and counterexamples. All formulas use consistent units and must be matched to a valid market convention.

| Concept | Formula | Interpretation / caveat |
|---|---|---|
| Expected value | $E[X]=\sum_i p_i x_i$ | Payoffs weighted by event probabilities |
| Variance | $Var(X)=E[(X-E[X])^2]$ | Dispersion around mean |
| Bayes | $P(H\mid S)=P(S\mid H)P(H)/P(S)$ | Requires base rate and likelihood |
| Simple return | $r_t=P_t/P_{t-1}-1$ | Adjust dividends/actions as appropriate |
| Log return | $g_t=\ln(P_t/P_{t-1})$ | Additive over contiguous periods |
| Compound return | $R=\prod_t(1+r_t)-1$ | Arithmetic returns cannot just be summed |
| Sample mean SE, IID | $SE(\bar x)=s/\sqrt{n}$ | IID assumption often too strong for market returns |
| Annualized vol | $s_d\sqrt{N}$ | Uses assumed sessions/year; estimate not guarantee |
| Sharpe (simplified) | $\sqrt N\,\bar r_e/s(r_e)$ | Ex-post, assumes defensible frequency/scaling |
| Maximum drawdown | $\min_t[V_t/\max_{s\le t}V_s-1]$ | Minimum drawdown value (nonpositive) |
| Portfolio expected return | $E[r_p]=w^T\mu$ | Depends on uncertain expected return estimates |
| Portfolio variance | $\sigma_p^2=w^T\Sigma w$ | Covariance stability is an assumption |
| Futures carry (idealized) | $F_0=S_0e^{(r-q)T}$ | Funding/dividend assumptions, market frictions |
| Call expiry payoff | $\max(S_T-K,0)$ | Excludes premium and fees |
| Put expiry payoff | $\max(K-S_T,0)$ | Excludes premium and fees |
| Call profit per unit | $\max(S_T-K,0)-c_0$ | Before fees, adjusted for position size |
| Put profit per unit | $\max(K-S_T,0)-p_0$ | Before fees, adjusted for position size |
| Midprice | $(bid+ask)/2$ | Does not imply execution at midpoint |
| Relative spread | $(ask-bid)/mid$ | Quoted spread, excludes impact |
| Standardized signal | $z=(x-\mu_{past})/\sigma_{past}$ | Past-only estimates; guard near-zero denominator |
| Naive vol target | $w=\sigma_{target}/\hat\sigma$ | Must cap risk, leverage and transaction costs |
| Capacity estimate | $bytes\approx events/s\times bytes/event\times seconds$ | Raw estimate before metadata/compression |
| File explosion | $files=instruments\times dates$ | Avoid one-file-per-contract-day designs |

## BSM (pedagogical European-call model)

$$C=Se^{-qT}N(d_1)-Ke^{-rT}N(d_2)$$
$$d_1={\ln(S/K)+(r-q+\sigma^2/2)T\over\sigma\sqrt T},\quad d_2=d_1-\sigma\sqrt T$$

**Cautions:** continuous yield and lognormal assumptions; model outputs are **derived**, not exchange facts. Greeks are partial derivatives with respect to chosen model inputs. Indian instrument specifics and settlement rules must be checked independently.

## Formula validation habit

For every formula, write: variable definitions; units; assumptions; domain; degenerate cases; a hand-calculated sanity test; what inputs are known at decision time; and where execution costs enter. A mathematically correct formula used with future data is still an invalid trading backtest.
