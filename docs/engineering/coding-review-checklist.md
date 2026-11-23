# Coding and review standards

## Domain

- Use explicit value types for identity, datetime, price, quantity and effective dates.
- No provider SDK, SQL or CLI imports inside the financial domain.
- Invariants return meaningful failures, not implicit defaults.
- Source payload parsing cannot silently coerce malformed prices or unknown identifiers.
- Unit tests cover boundary values: strike zero, expiry order, unusual symbols, duplicate IDs.

## Application

- A use case expresses its inputs, outputs, side effects and failure states.
- Dependencies arrive through explicit interfaces; no hidden global registry.
- Checkpoint transition cannot move ahead of durable commit.
- Job retry and cancellation behavior is documented.

## Storage

- Staging and committed artifacts are distinguishable.
- Published Parquet files are immutable; checksum verified.
- SQLite migrations tested against previous schemas.
- Queries read explicit dataset releases and do not include staging files.

## Data ethics and rights

- Source provenance and data license/use conditions are recorded.
- Never log credentials or upload proprietary raw data to a third party without authorization.
- No assumption that visible market data grants automatic collection rights.

## Pull request format

```text
Problem:
Chosen approach / rejected alternatives:
Data contract changes:
New assumptions:
Failure and recovery behavior:
Tests run (including exact command/result):
Performance evidence:
Docs/ADR changes:
Known limitations:
```

**Review gate:** anyone approving financial code must explain what prices/quantities represent and whether each input is available at decision time.
