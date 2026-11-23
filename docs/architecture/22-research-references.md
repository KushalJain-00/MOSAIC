# 22 — Research Notes and References

**Research snapshot date: 16 November 2026.** These sources informed architectural patterns and examples. They are not endorsements or proof that the user's own deployment is compliant, profitable or entitled to the same capabilities. Vendor documentation changes; check the current version at implementation time.

## 1. Trading-system architecture patterns

### NautilusTrader architecture
https://nautechsystems.github.io/nautilus_docs/latest/concepts/architecture/

Useful concepts observed: domain-driven design, event-driven messaging, publish/subscribe and request/response, ports-and-adapters, crash-only design, explicit DataEngine/ExecutionEngine/RiskEngine/Portfolio responsibilities, caches of instruments/orders/positions, execution reconciliation, and shared kernel interfaces across backtest, sandbox and live environments. This supports our separation of core trading semantics from provider/venue adapters and our requirement that simulated and live modes share contracts.

### QuantConnect LEAN Algorithm Framework
https://www.quantconnect.com/docs/v2/writing-algorithms/algorithm-framework/overview

Useful decomposition: universe selection → alpha/signal generation → portfolio construction → risk management → execution. It reinforces the distinction between insights/signals and portfolio targets and between portfolio construction and execution.

### QuantConnect reality models
https://www.quantconnect.com/docs/v2/writing-algorithms/reality-modeling/key-concepts

Useful warning: backtest results depend on fill, slippage, fee, buying-power, settlement and other reality models. Our backtest architecture therefore pins such model versions and requires cost/market-mechanics assumptions in every report.

## 2. Market-data architecture and recovery

### KX / kdb+ streaming architecture
https://code.kx.com/kdb-x/how_to/manage_streaming_data/architecture.html

Reference pattern: feed handlers publish to a tickerplant/message log; consumers such as intraday and historical databases process streams. This informed the distinction between raw feed capture, durable live distribution and long-term historical analytics. We use the pattern conceptually, not as a requirement to adopt kdb+.

### Binance Spot WebSocket streams
https://github.com/binance/binance-spot-api-docs/blob/master/web-socket-streams.md

Important implementation example: local order-book reconstruction uses buffered incremental updates plus a REST snapshot and update-ID bridging; if a sequence gap occurs, the local book must be discarded and rebuilt. The docs also describe websocket lifetime and heartbeat behavior. This is why sequence recovery belongs in a venue-specific adapter and why a socket reconnect alone is not proof of a valid book.

### Coinbase Exchange WebSocket feed documentation
https://help.coinbase.com/en/developer-platform/websocket-feeds/exchange

Useful for comparing channel semantics and Level 2 snapshot/update handling. Each venue's own documentation must define the actual recovery algorithm for that adapter.

### CME MDP recovery documentation
https://cmegroupclientsite.atlassian.net/wiki/spaces/EPICSANDBOX/pages/457672425/MDP+3.0+-+MBP+and+MBOFD+Market+Recovery

Illustrates sequence/packet-gap detection and market recovery concepts for exchange market-data protocols.

## 3. Order management and protocol semantics

### FIX Trading Protocol — Trade specification
https://www.fixtrading.org/wp-content/uploads/download-manager-files/FIX-Latest-Specification-Trade.pdf

Important distinction: execution report event type (`ExecType`) and current order status (`OrdStatus`) are separate semantics; pending cancel/replace is not confirmed canceled/replaced. This informs OMS state design and the requirement to retain native status/event detail.

### FIX Trading Community standards
https://www.fixtrading.org/standards/fix-protocol/

Useful protocol standardization context for order, execution and market-data messages. Actual broker implementations may support subsets or extensions; validate each counterparty's rules of engagement.

### Nasdaq OUCH specifications
https://nasdaqtrader.com/content/technicalsupport/specifications/TradingProducts/Ouch5.0.pdf

Useful exchange-specific example of order-entry state and messages including new, replace, cancel, mass cancel, accepted/replaced/canceled/executed/rejected and pending outcomes. Demonstrates why a generic broker API contract must preserve venue-specific semantics.

## 4. Event delivery and data storage

### AWS Transactional Outbox pattern
https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html

Useful warning about dual-write failure between a DB transaction and event publication; duplicate notifications and ordering require idempotent consumers. This informs outbox/inbox and replay rules.

### Confluent Kafka consumer documentation
https://docs.confluent.io/platform/current/clients/consumer.html

Reference for consumer groups, offsets, replay and delivery/commit semantics. Exactly-once claims must be understood within their transaction boundaries and do not eliminate uncertain external broker side effects.

### Apache Parquet format
https://parquet.apache.org/docs/overview/

Reference for a column-oriented analytical file format; informed use of Parquet for historical analytical datasets while keeping operational state in a transactional relational database.

### OpenTelemetry signals
https://opentelemetry.io/docs/concepts/signals/

Reference for metrics, traces and logs as complementary observability signals. Our platform adds trading-specific domain telemetry and audit records.

### Google SRE monitoring distributed systems
https://sre.google/sre-book/monitoring-distributed-systems/

Reference for latency, traffic, errors and saturation (the “golden signals”) as useful service health dimensions. We extend these with data freshness, risk utilization, order uncertainty and reconciliation state.

## 5. Risk controls and market access

### SEC Rule 15c3-5 market access FAQ
https://www.sec.gov/rules-regulations/staff-guidance/trading-markets-frequently-asked-questions/divisionsmarketregfaq-0

US broker-dealer market-access reference. It informs the general engineering principle of pre-trade financial/regulatory controls and controlled access, but it does not apply universally to all markets/accounts and is not a substitute for Indian or other local requirements.

### SEBI — Safer participation of retail investors in Algorithmic trading (4 February 2025)
https://www.sebi.gov.in/legal/circulars/feb-2025/safer-participation-of-retail-investors-in-algorithmic-trading_91614.html

Primary Indian regulatory reference for retail participation in algorithmic trading.

### SEBI — timeline extension (30 September 2025)
https://www.sebi.gov.in/legal/circulars/sep-2025/extension-of-timeline-for-implementation-of-sebi-circular-dated-february-04-2025-on-safer-participation-of-retail-investors-in-algorithmic-trading-_96979.html

Relevant timeline/implementation update.

### NSE — Empanelled algo providers
https://www.nseindia.in/static/trade/empanelled-algo-providers-exchange

Current NSE reference page including reference to NSE/INVG/73992 dated 30 April 2026. Verify latest implementation standards, FAQs, circular updates and broker obligations before Indian live deployment.

### NSE — algo-trading / platform services page
https://www.nse.in/static/trade/platform-services-non-neat-decision-support-tools-algorithm-trading

Reference for NSE platform services and algorithm-trading-related information.

## 6. Architectural conclusions drawn from the research

1. Use bounded domains and ports/adapters; do not tie strategy logic to vendor SDKs.
2. Share a trading kernel across replay/backtest/paper/live while changing only the relevant data/execution adapters and environment policies.
3. Separate market-data capture, sequence recovery, normalization, durable distribution and historical storage.
4. Treat each venue's sequence and snapshot rules as adapter responsibilities.
5. Separate target construction, risk authorization, OMS state, execution facts and accounting.
6. Use durable events, idempotency, outbox/inbox and replay for critical state transitions.
7. Model fill, fee, slippage, buying power, settlement and margin assumptions explicitly in backtests.
8. Make operational reconciliation, monitoring and fail-closed behavior part of the trading architecture, not optional add-ons.
9. Treat market-access and licensing compliance as live-release gates with per-market policies.

## 7. Limits of this research snapshot

This reference set establishes robust architectural patterns, not a complete provider comparison or legal review. Provider selection must be researched separately against exact instruments, historical depth, real-time entitlements, latency, API quotas, commercial/non-display/model-training rights, price and broker compatibility. For each chosen provider/venue, save the exact versioned API documentation, terms, tested payload fixtures and capability matrix in the project. Re-verify current documentation before integration and periodically thereafter.
