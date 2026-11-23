# Combined learning + engineering roadmap

The ultimate MOSAIC vision spans global markets and autonomous research, but the near-term proof is a trustworthy local Indian EOD platform.

| Phase | Engineering deliverable | Learning gate |
|---|---|---|
| M0 | uv project, tests, CLI skeleton, ADRs | Python types, dependency direction, fixtures |
| M1 | Historical instrument registry | contract identity, Indian market mechanics |
| M2 | Qualify sources; permitted local import | data rights, versioned parsing, lineage |
| M3 | Raw archive → canonical Parquet | financial validation, timestamps, storage |
| M4 | Crash recovery, idempotency, revisions | transaction boundaries, state machines |
| M5 | Coverage and source-aware quality | sparse observations, market calendars |
| M6 | DuckDB/CLI research queries | partitions, projections, reproducibility |
| M7 | Benchmarks, CI, release evidence | independent explanation of full flow |
| Later | Live / intraday / L2/L3 / dashboard / strategies | microstructure, ML, independent risk, execution |

## Suggested ordering of lessons

1. Foundations 01–03; Indian markets 01–03; systems engineering 01.
2. Data engineering 01–02; Indian markets 04–05; systems engineering 03–04.
3. Time series 01–04; data engineering 03–05; systems engineering 02,05.
4. Strategy research 01–05 and portfolio foundations 04.
5. Mixed assessment then capstone.

**Do not confuse dependency with schedule:** lessons may be interleaved, but advance a critical implementation only when its prerequisites can be defended on a novel scenario.
