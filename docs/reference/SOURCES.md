# Source list and verification policy

## Primary and vendor documentation used in this pack

- Jane Street [Probability & Markets](https://www.janestreet.com/probability-markets/) and [Puzzles](https://www.janestreet.com/puzzles/) — learning inspiration; **not** the source of the user's uploaded tutor skill.
- NSE [derivatives reports](https://www.nseindia.com/all-reports-derivatives) and [historical reports](https://www.nseindia.com/static/resources/historical-reports-capital-market-daily-monthly-archives) — report families; availability and rights require re-verification.
- NSE Clearing [UDiFF specifications](https://www.nseclearing.in/udiff) — standardized format references.
- SEBI [equity derivatives source index](https://www.sebi.gov.in/curation/equity_derivatives.html) — official links to historical market-data resources.
- CFA Institute [Option valuation](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/pricing-valuation-options), [Contingent claims](https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/valuation-contingent-claims) — derivative pricing concepts.
- Options Industry Council [Volatility and Greeks](https://prd-web.optionseducation.org/advancedconcepts/volatility-the-greeks).
- DuckDB [Parquet](https://duckdb.org/docs/stable/data/parquet/overview) and [tips](https://duckdb.org/docs/current/data/parquet/tips) — read pruning and file layout.
- Python [asyncio](https://docs.python.org/3/library/asyncio.html), [concurrent futures](https://docs.python.org/3/library/concurrent.futures.html), [dataclasses](https://docs.python.org/3/library/dataclasses.html).
- SQLite [transactions](https://www.sqlite.org/lang_transaction.html).
- pytest [good practices](https://docs.pytest.org/en/stable/explanation/goodpractices.html).

## Interpretation rules

- Mathematics in these docs is standard pedagogical material, not individualized investing guidance.
- Real exchange contract rules, instrument availability and data-source terms change; re-check relevant official documents and dates before adapters or trading.
- An externally cited conceptual description does not verify a specific provider's API coverage or automation permission.
- A third-party library may expose a market-data endpoint while the underlying provider imposes separate contractual restrictions.
