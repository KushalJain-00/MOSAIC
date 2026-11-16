# Architecture document index

Start at [00 — Master Architecture Overview](docs/architecture/00-architecture-overview.md).

| # | Document | Main question answered |
|---:|---|---|
| 00 | [Master Architecture Overview](docs/architecture/00-architecture-overview.md) | What is the complete system and how do all planes connect? |
| 01 | [Market Taxonomy and Instrument Registry](docs/architecture/01-market-taxonomy-instrument-registry.md) | How do we uniquely identify listings, contracts, venues and universes? |
| 02 | [Universal Market-Data Platform](docs/architecture/02-universal-market-data-platform.md) | How do we collect, recover, normalize and validate every data family? |
| 03 | [Storage, Lineage and Dataset Lifecycle](docs/architecture/03-storage-lineage-datasets.md) | Where does each kind of data live and how is it reproducible? |
| 04 | [Event Backbone and Messaging Semantics](docs/architecture/04-event-backbone-messaging.md) | How do components communicate reliably? |
| 05 | [Feature and Market Intelligence](docs/architecture/05-feature-market-intelligence.md) | How are point-in-time features and context computed? |
| 06 | [Research and Agent Platform](docs/architecture/06-research-agent-platform.md) | How do agents research and propose changes safely? |
| 07 | [Strategy Runtime and Signals](docs/architecture/07-strategy-runtime-signals.md) | How do independent strategy instances run and emit targets? |
| 08 | [Backtesting and Validation](docs/architecture/08-backtesting-validation.md) | How do we test strategies with realistic assumptions? |
| 09 | [Portfolio and Capital Accounting](docs/architecture/09-portfolio-capital-accounting.md) | How does hybrid allocation work across virtual and actual books? |
| 10 | [Independent Risk Authority](docs/architecture/10-independent-risk-authority.md) | Who has final permission to take risk? |
| 11 | [OMS and Execution](docs/architecture/11-oms-execution.md) | How are orders, acknowledgements, fills, cancels and corrections managed? |
| 12 | [Broker and Venue Adapters](docs/architecture/12-broker-venue-adapters.md) | How are broker/exchange differences isolated? |
| 13 | [Database and Entity Model](docs/architecture/13-database-entity-model.md) | What are the main entities and relationships? |
| 14 | [Interface and Event Contracts](docs/architecture/14-interface-event-contracts.md) | What exact information crosses each boundary? |
| 15 | [End-to-End Flows](docs/architecture/15-end-to-end-flows.md) | How does each complete workflow execute? |
| 16 | [Operations, Observability and Security](docs/architecture/16-operations-observability-security.md) | How do humans operate and secure the system? |
| 17 | [Deployment and Scaling](docs/architecture/17-deployment-scaling.md) | How does logical architecture map to real processes and infrastructure? |
| 18 | [Failure Modes and Runbooks](docs/architecture/18-failure-modes-runbooks.md) | What happens when components fail? |
| 19 | [Test Strategy and Roadmap](docs/architecture/19-test-strategy-roadmap.md) | How is the architecture implemented and proven? |
| 20 | [Regulation and Market Access](docs/architecture/20-regulatory-market-access.md) | What must be verified before market access and live operation? |
| 21 | [Decisions and Open Questions](docs/architecture/21-decisions-assumptions-open-questions.md) | What is agreed, assumed or still unresolved? |
| 22 | [Research References](docs/architecture/22-research-references.md) | Which real systems and official sources informed the design? |
