# Synthetic teaching charts

Charts in this folder are **original, mathematically illustrative examples**, not claims about live Indian markets or investment returns.

- `option-payoffs-synthetic.png`: long call and put expiry payoffs before fees, assumed strikes/premiums.
- `volatility-regime-synthetic.png`: seeded Gaussian synthetic returns with a variance regime shift and lagging rolling estimate.
- `bayes-base-rates-synthetic.png`: sensitivity of posterior precision to the false-positive rate, for a hypothetical event with 10% prevalence.

Source generation: `generate_charts.py` shipped with this documentation pack; rerunnable with NumPy and Matplotlib. Figure source parameters and caveats are defined there.
