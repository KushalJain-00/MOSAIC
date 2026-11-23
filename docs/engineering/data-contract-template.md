# Contract: <name> (version X.Y)

**Owner:** ___ · **Consumers:** ___ · **Source:** ___ · **Status:** proposed/approved/deprecated

## Schema

| Field | Type | Units | Nullable | Semantic meaning | Validity rules |
|---|---|---|---|---|---|
| instrument_id | string | none | no | permanent MOSAIC identity | resolvable to correct historical record |
| event_time | timestamptz | UTC | conditional | source market event time | preserve original precision |

## Semantics and time

Explain `event_time`, `published_at`, `ingested_at`, session timezone and lifecycle. How is an absent observation represented? Does the source supply final or preliminary values?

## Compatibility

Compatible additions, breaking changes, version negotiation and parser quarantine rules.

## Validation and examples

Provide valid and invalid *synthetic* fixtures with expected error codes and tests. Do not use unverified sample exchange rows as authoritative schema examples.

## Failure behavior

What happens if input is missing, duplicated, late, revised, out of order or unrecognized?

## Evidence

Source specification URL, publication date, rights statement, linked tests and approval.
