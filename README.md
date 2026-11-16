# Multi-Market Quantitative Trading Platform — Architecture Blueprint

**Version:** 0.1 (architecture baseline)  
**Prepared:** 16 November 2026  
**Scope:** Multi-market platform designed from the outset for Indian and international equities, crypto, FX, metals and other commodities, futures, options and later additional instrument/data families.

This repository is the architecture source of truth. It describes the logical system, how subsystems connect, the information that crosses each boundary, the reasons for those boundaries, failure behavior, security and governance requirements, and an implementation sequence. It is deliberately more than a collection of box diagrams.

## Read this first

1. [Architecture overview and master system diagram](docs/architecture/00-architecture-overview.md)
2. [Market taxonomy and instrument registry](docs/architecture/01-market-taxonomy-instrument-registry.md)
3. [Universal market-data platform](docs/architecture/02-universal-market-data-platform.md)
4. [Storage, lineage and dataset lifecycle](docs/architecture/03-storage-lineage-datasets.md)
5. [Event backbone and messaging semantics](docs/architecture/04-event-backbone-messaging.md)
6. [Feature and market-intelligence platform](docs/architecture/05-feature-market-intelligence.md)
7. [Research and agent platform](docs/architecture/06-research-agent-platform.md)
8. [Strategy runtime and signal contracts](docs/architecture/07-strategy-runtime-signals.md)
9. [Backtesting, replay and validation](docs/architecture/08-backtesting-validation.md)
10. [Portfolio, capital allocation and accounting](docs/architecture/09-portfolio-capital-accounting.md)
11. [Independent risk authority](docs/architecture/10-independent-risk-authority.md)
12. [Order management and execution lifecycle](docs/architecture/11-oms-execution.md)
13. [Broker and venue adapter architecture](docs/architecture/12-broker-venue-adapters.md)
14. [Database and entity model](docs/architecture/13-database-entity-model.md)
15. [Interface and event contracts](docs/architecture/14-interface-event-contracts.md)
16. [End-to-end sequence diagrams](docs/architecture/15-end-to-end-flows.md)
17. [Operations, observability and security](docs/architecture/16-operations-observability-security.md)
18. [Deployment, runtime and scaling](docs/architecture/17-deployment-scaling.md)
19. [Failure modes and operational runbooks](docs/architecture/18-failure-modes-runbooks.md)
20. [Test strategy and implementation roadmap](docs/architecture/19-test-strategy-roadmap.md)
21. [Regulation, licensing and market access](docs/architecture/20-regulatory-market-access.md)
22. [Architecture decisions, assumptions and open questions](docs/architecture/21-decisions-assumptions-open-questions.md)
23. [Research notes and references](docs/architecture/22-research-references.md)

## Design intent

- **All target markets belong to one architecture.** Indian equities and crypto are useful first integration tests, not architectural silos or permanent priorities.
- **One canonical domain model, multiple market-specific adapters.** Do not flatten materially different market mechanics into false uniformity.
- **Research and production trading are separate trust zones.** Agents may investigate and propose; they cannot directly enable, fund or authorize live trading.
- **The risk authority is independent.** Strategy and allocation components propose intent; a separately governed control layer can reject it.
- **The OMS owns order state; the broker owns the authoritative external execution facts.** The internal system records and reconciles both.
- **Backtest, shadow, paper and live share strategy semantics and contracts.** Only the data and execution environment should vary.
- **Event-driven inside clear bounded contexts; not microservices by default.** Start with explicit module boundaries and durable interfaces; distribute components only when justified.
- **Point-in-time correctness is mandatory.** Every research result must be reproducible from the exact data, features, code, parameters and cost assumptions available at that time.

## What this blueprint is — and is not

This is a detailed logical architecture baseline and engineering design. It is not a claim that provider accounts, market-data licenses, broker integrations, exchange approvals, production latency, profitability or regulatory permissions have already been obtained. Those are external facts and must be verified for each intended venue, broker, jurisdiction, product and deployment model.

The diagrams specify responsibility and information flow. A logical box does **not** automatically mean a separate process, container, server or microservice. Initial deployment should favour a modular application plus isolated workers and robust persistence. A true low-latency/HFT architecture would need a separate measured design and potentially native components, co-location and exchange-specific connectivity.

## Diagram conventions

- Solid arrows: data or control flow.
- Dotted arrows: governance, configuration, observation or optional relationship.
- A **command** requests a state change; an **event** records a fact that has occurred; a **query** reads a projection.
- A **target** expresses desired exposure; an **order** is an instruction submitted to a broker/venue; an **execution** is a reported fill. These must never be conflated.
- Mermaid diagrams are embedded in Markdown and can be edited/versioned with the documents.

## Recommended use

Review the overview first, then read the subsystem documents in order. Treat Section 21 as the decision register. Before implementation, convert unresolved items into explicit decisions with owners, evidence and acceptance tests. Keep the interface contracts stable enough for parallel work, but version them deliberately when the domain model changes.
