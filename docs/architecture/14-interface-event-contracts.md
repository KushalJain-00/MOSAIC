# 14 — Interface and Event Contracts

## 1. Contract-first principle

Every subsystem communicates through versioned domain contracts, not shared mutable Python objects or provider-specific payloads. The contracts below are the conceptual baseline. Before implementation, define exact JSON Schema/Protobuf/Avro/Pydantic models, decimal conventions, required/null fields, enum values and compatibility rules in `/schemas`.

## 2. Key interfaces

| Producer | Consumer | Interface / contract | Why |
|---|---|---|---|
| Provider adapter | Ingestion/normalizer | Raw provider message + source metadata | Preserve provenance and native facts |
| Registry | Data/strategy/OMS | Instrument/listing/spec version | Shared identity and correct trading constraints |
| Data platform | Event backbone | Canonical market/reference event | Fan-out and replay |
| Feature engine | Strategy runtime | Feature snapshot with as-of and quality | Consistent point-in-time inputs |
| Strategy runtime | Portfolio | Signal and target intent | Separate analysis from target construction |
| Portfolio allocator | Risk | Proposed order/target exposure + snapshot | Centralized capital coordination |
| Risk authority | OMS | Decision + reservation token + policy version | Independent permission gate |
| OMS | Broker adapter | Broker-neutral order command | Isolate vendor protocol |
| Broker adapter | OMS | Native acknowledgement/reject/execution/correction mapped event | Preserve external facts |
| OMS/ledger | Portfolio | Fill/fee/position/cash facts | Reconcile virtual and actual views |
| Research lab | Release registry | Validation package + release proposal | Controlled promotion |

## 3. Instrument contract (illustrative)

```json
{
  "instrument_id": "canonical-instrument-id",
  "listing_id": "venue-listing-id",
  "venue_id": "venue-id",
  "asset_class": "equity|crypto|fx|commodity|future|option|bond|other",
  "instrument_type": "typed-product-kind",
  "symbol": "display-symbol",
  "base_currency": "optional-currency",
  "quote_currency": "optional-currency",
  "settlement_currency": "optional-currency",
  "underlying_instrument_id": null,
  "contract_spec_version_id": "effective-contract-spec-id",
  "calendar_id": "trading-calendar-id",
  "tradability_state": "tradable|suspended|expired|delisted|unknown",
  "valid_from": "utc-instant",
  "valid_to": null,
  "source_refs": []
}
```

Typed contract extensions carry equity share-class data, crypto contract/funding/margin fields, FX pair conventions, futures expiry/multiplier/delivery, or options strike/right/style/settlement. Do not force all asset-class fields into one untyped bag without schema validation.

## 4. Market data event (illustrative)

```json
{
  "event_id": "stable-event-id",
  "event_type": "market.bar.v1",
  "schema_version": 1,
  "instrument_id": "canonical-id",
  "listing_id": "listing-id",
  "venue_id": "venue-id",
  "provider_id": "provider-id",
  "timeframe": "1m",
  "bar_start": "utc-instant",
  "bar_end": "utc-instant",
  "event_time": "utc-instant",
  "received_at": "utc-instant",
  "available_at": "utc-instant",
  "is_final": true,
  "revision": 1,
  "open": "decimal-string",
  "high": "decimal-string",
  "low": "decimal-string",
  "close": "decimal-string",
  "volume": "decimal-string",
  "currency": "currency-code",
  "quality": {"status": "valid", "sequence_state": "not-applicable"},
  "raw_payload_ref": "object-reference"
}
```

Use event-specific contracts for trade, quote, order book snapshot/delta, funding, option chain, corporate action and macro/news facts. A generic OHLCV payload is insufficient for all market types.

## 5. Feature snapshot contract

Fields: `snapshot_id`, `instrument_id/listing_id`, `feature_set_version`, `as_of`, `available_at`, `source_watermark`, `timeframe`, `values` (typed map or schema-defined record), per-feature missing/quality status, upstream dataset IDs and computation version. Strategy must be able to request a snapshot strictly as-of a decision instant.

## 6. Target intent contract

Fields: `target_id`, `strategy_instance_id`, `strategy_release_id`, `virtual_book_id`, `instrument_id`, `account_scope`, `target_kind` (units/notional/weight/delta exposure), `target_value`, `currency/unit`, `current_exposure_snapshot_id`, `valid_from`, `expires_at`, priority, `confidence_definition`, forecast horizon, reason codes, input snapshot IDs, idempotency key and supersedes/cancel relationship.

## 7. Risk request and decision contract

Request includes proposal ID, account and instrument IDs, side, quantity, price/price type, order type, time-in-force, strategy attribution, current position/open-order snapshot, data-quality snapshot, expected worst-case exposure and idempotency key. Decision includes decision ID, action, policy version, risk snapshot, reservation token, permitted quantity/price boundaries, expiry and machine-readable reason codes.

## 8. OMS order command and execution event

Order command includes internal parent/child IDs, stable client order ID, broker/account/venue, instrument, side, quantity, order type, limit/stop fields, time-in-force, execution instruction, reservation ID, strategy attribution and idempotency key. Execution report event includes broker order ID, client order ID, external execution ID, event/status type, last and cumulative quantities, last and average price, leaves quantity, trade/settlement time, liquidity flag if available, native status, source message, correction/bust linkage and raw report reference. Commission/fee may be a separate event.

## 9. Error and status contract

Errors have `category`, `retryability`, `scope`, `native_code`, `native_message`, `correlation_id`, `remediation_hint` and `safe_action`. Do not infer retryability from HTTP status alone. Use domain enums for stale data, entitlement, sequence gap, risk rejection, broker rejection, submission unknown and reconciliation required.

## 10. Schema lifecycle

- Schema registry or equivalent validates producers/consumers in CI.
- Additive optional fields are the default; breaking meaning changes create a new version.
- Consumers reject unknown required semantics safely.
- Contract fixtures include real/sanitized venue payloads and edge cases.
- Schema and example versions are tied to release commits.

A first pass of machine-readable schemas should be created in `/schemas` only after field-level ownership and required semantics are agreed; do not pretend illustrative snippets here are production-ready schemas.
