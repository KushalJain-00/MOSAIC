# 21 — Architecture Decisions, Assumptions and Open Questions

## 1. Decisions established by current design discussion

| ID | Decision | Rationale | Status |
|---|---|---|---|
| D-001 | Design for all target markets together from day one | Shared canonical architecture avoids later redesign; market-specific adapters preserve differences | Accepted |
| D-002 | Hybrid strategy organization: independent instances and virtual books with central coordination | Strategy autonomy plus global risk/portfolio view | Accepted |
| D-003 | Hybrid capital allocation: hard risk boundaries with dynamic allocation inside | Limits uncontrolled capital shift while adapting to validated performance | Accepted |
| D-004 | LLM/research plane separated from latency-critical execution | Variable latency and prompt/runtime risk are inappropriate for critical order path | Accepted |
| D-005 | Agents may propose experiments/releases but cannot self-approve live | Prevents unvalidated self-modification | Accepted |
| D-006 | Strategy emits signals/targets, not broker orders | Central portfolio/risk/OMS ownership | Accepted |
| D-007 | One actual broker ledger plus internal strategy virtual attribution | Avoids treating simulated books as actual segregated accounts | Accepted |
| D-008 | Shared data/features and event routing; no process per combination | Avoid combinatorial infrastructure explosion | Accepted |
| D-009 | CSV is export, not sole source of truth | Better lineage, concurrency and analytical storage | Accepted as recommendation; final preference not yet confirmed |
| D-010 | Logical architecture first, modular implementation before microservices | Keep operating complexity proportional to measured needs | Recommended baseline |

## 2. Explicit assumptions for this blueprint

- The first implementation is research-first and may later be authorized for live trading; production live trading is not presumed enabled.
- Indian equities and crypto may be first end-to-end test cases, but the identity model, contracts and subsystem boundaries cover all target markets.
- The operator may initially be one person/small team; high availability and 24/7 operations capabilities are not assumed.
- Python is a practical initial language for research and orchestration; hot paths can be replaced/isolated after measurement.
- A relational DB and Parquet-based lake are starting recommendations, not immutable technology commitments.
- Broker accounts may net trades; virtual books are attribution ledgers unless true segregated accounts exist.
- Provider coverage, latency, historical depth and licensing are not assumed until verified.

## 3. Open decisions that materially change implementation

### Trading account and business model
1. Is this strictly personal/proprietary trading, or will strategies/capabilities be offered to others?
2. Which broker(s), exchange(s), account types and product permissions are likely candidates?
3. Will there be one central account, multiple accounts, family/client accounts or managed allocations?
4. Is the desired execution model broker API only, FIX/DMA where permitted, or venue-specific direct connectivity?

### Markets and products
5. Which exact segments are in scope first: NSE cash/F&O, BSE, international cash, crypto spot/perpetuals, FX spot/CFDs/futures, commodity futures/options, etc.?
6. Which instruments are prohibited or excluded (illiquid, penny stocks, exotic derivatives, restricted tokens, leverage products)?
7. Are short selling, margin, leverage, derivatives exercise and physical delivery in scope?

### Horizons and latency
8. Are initial intraday decisions completed 1m/5m bars, trade/quote events, or full order-book events?
9. What end-to-end latency budget is actually required, measured from provider event to broker acknowledgement?
10. Is true HFT/co-location explicitly out of scope for the first architecture release?

### Data and licensing
11. Which data vendors have budget/entitlements, and is commercial/non-display/model-training usage needed?
12. Are historical tick/depth data and option-chain history required, or is OHLCV enough for initial research?
13. What retention, storage and data deletion requirements apply?
14. What is expected universe size per market, timeframe and strategy family?

### Capital and risk
15. What capital base/currency and account structure should risk budgets use?
16. Which hard limits are mandatory: max drawdown, per-day loss, per-strategy loss, gross/net exposure, leverage, instrument/sector/currency limits, margin reserve?
17. How should manual positions or trades outside this system be represented in consolidated risk?
18. Who can authorize live enablement, policy change, kill-switch reset and capital overrides?

### Operations and deployment
19. Local workstation, home server, cloud VM, managed cloud or hybrid deployment?
20. Expected uptime and RPO/RTO requirements?
21. What is the operational budget for databases, data vendors, compute and monitoring?
22. Is multi-user access needed from the first version?

## 4. Items that must remain configurable rather than hardcoded

Markets/venues/instruments; calendars and contract specs; provider endpoints/quotas/coverage; timeframes; strategy subscriptions; cost models; account mappings; risk limits; execution algorithms; data-retention and license policies; market-access rules; model versions; feature definitions; environment toggles and rollout thresholds.

## 5. Deliberately deferred decisions

- Exact provider and broker shortlist after capability, licensing and cost verification.
- Exact streaming broker and database topology after throughput estimates.
- Native language components and low-latency/HFT requirements after measured latency tests.
- Advanced smart order routing and cross-venue arbitrage until market access and order-book data are validated.
- Production ML/LLM hosting and GPU choice after model workloads are known.
- Detailed physical SQL schema and retention/partition design after expected data volume.

## 6. Review rule

Any architectural change must record: problem; affected boundaries; alternatives; impact on contracts, safety, performance, operations and licensing; migration plan; test evidence; decision owner; date; status. Avoid implementing large changes based only on an agent-generated “best practice” claim.
