# 15 — End-to-End Sequence Diagrams and Traceability

These traces explain how information and authority move across module boundaries. Each sequence is an acceptance-test candidate. IDs/correlation links must be retained across every hop so an operator can reconstruct one decision or order without searching uncorrelated log text.

## 1. New instrument discovery to usable dataset

```mermaid
sequenceDiagram
  autonumber
  participant D as Discovery scheduler
  participant R as Instrument registry
  participant V as Venue reference source
  participant P as Provider capability catalog
  participant I as Ingestion scheduler
  participant A as Adapter
  participant Q as Quality service
  participant L as Dataset catalog/lake
  D->>R: Discover listing or contract family
  R->>V: Fetch native reference metadata
  V-->>R: Listing, contract, session and lifecycle facts
  R->>R: Canonical identity + effective-dated spec
  R->>P: Resolve provider mappings, data families and rights
  P-->>R: Verified/unknown capabilities and limitations
  R->>I: Emit listing-created / metadata-updated event
  I->>A: Schedule supported historical chunks
  A->>L: Persist raw request and response
  A->>Q: Normalize and validate records
  Q->>L: Accepted dataset version + quarantine report
  Q-->>R: Coverage, quality and entitlement state
  R->>R: Set data readiness (not necessarily tradable)
```

## 2. Live market event to strategy decision

```mermaid
sequenceDiagram
  autonumber
  participant V as Venue/provider
  participant A as Venue adapter
  participant G as Sequence/quality gate
  participant B as Event backbone
  participant F as Feature engine
  participant R as Strategy event router
  participant S as Strategy worker
  participant D as Decision store
  V-->>A: Trade/quote/book event
  A->>A: Preserve raw payload, source time and sequence
  A->>G: Normalize using provider mapping version
  G->>G: Validate sequence, freshness, session and quality
  alt Valid event
    G->>B: Publish canonical event
    B->>F: Deliver to feature consumer group
    F->>F: Update base features and MTF context
    F->>B: Publish feature snapshot event
    B->>R: Deliver relevant event/features
    R->>S: Invoke subscribed instance in keyed order
    S->>S: Check warmup, watermark, data quality and freshness
    S->>S: Compute signal/target intent
    S->>D: Persist decision + input feature snapshot IDs
  else Invalid or gap detected
    G->>B: Publish data-quality/degraded-state event
    B->>R: Notify affected strategies
    R->>S: Pause/degrade or skip according to policy
    S->>D: Record skipped decision and reason
  end
```

## 3. Target to risk authorization to actual fill

```mermaid
sequenceDiagram
  autonumber
  participant S as Strategy runtime
  participant V as Virtual book
  participant P as Portfolio allocator
  participant R as Risk authority
  participant O as OMS
  participant A as Broker adapter
  participant B as Broker/venue
  participant L as Ledger/reconciliation
  S->>V: Submit target intent with attribution
  V->>P: Current virtual exposure + new target
  P->>P: Consolidate targets and calculate proposed size
  P->>R: RiskCheckRequest with account/position/open-order snapshot
  R->>R: Check policies and atomically reserve buying power/risk
  alt Approved
    R-->>O: Approval + constraints + reservation token
    O->>O: Persist parent/child order and outbox event
    O->>A: Submit stable client order ID and terms
    A->>B: Native order request
    B-->>A: Ack with broker order ID or rejection
    A-->>O: Normalized ack/reject event + native report ref
    B-->>A: Partial/final execution report(s)
    A-->>O: Execution event(s) with unique external IDs
    O->>L: Record execution facts and any separate fee reports
    L->>L: Book ledger and reconcile actual account state
    L-->>V: Update virtual attribution from actual fills
    L-->>P: Update consolidated portfolio projection
  else Denied / constrained
    R-->>P: Denial or reduced size + reason codes
    P-->>V: Record unfilled/rejected target allocation
  end
```

## 4. Submission timeout and unknown broker state

```mermaid
sequenceDiagram
  autonumber
  participant O as OMS
  participant A as Broker adapter
  participant B as Broker
  participant R as Reconciliation service
  O->>O: Persist child order + stable client ID
  O->>A: Submit order
  A->>B: Send native order request
  Note over A,B: Network breaks after broker may have received request
  A--xO: Timeout; no acknowledgement observed
  O->>O: Mark SUBMISSION_UNKNOWN; retain risk reservation
  O->>R: Reconcile by client ID / account / time window
  R->>B: Query open orders, order history and executions
  alt Broker order found
    B-->>R: Matching order / reports
    R-->>O: Bind broker ID and import current state
    O->>O: Resume normal event lifecycle
  else Broker conclusively reports no order
    B-->>R: No matching order, within supported query guarantees
    R-->>O: Resolution evidence
    O->>O: Retry only under explicit adapter policy and same idempotency key if supported
  else Query incomplete or conflicting
    B-->>R: Ambiguous state
    R-->>O: Remain unknown; escalate and block duplicate-risk action
  end
```

Do not assume a “not found” response proves no order if the broker's query endpoint is eventually consistent or has incomplete history. Adapter-specific reconciliation windows and guarantees must be documented.

## 5. Research proposal to eligible release

```mermaid
sequenceDiagram
  autonumber
  participant O as Research orchestrator
  participant A as Specialist agents
  participant S as Sandbox / experiment workers
  participant E as Experiment registry
  participant V as Independent validator
  participant H as Human approver
  participant R as Release registry
  participant T as Strategy runtime
  O->>A: Assign hypothesis and evidence task
  A->>S: Submit versioned code/config/data request
  S->>S: Run reproducible replay/backtest and resource-limited experiments
  S->>E: Store manifest, trials, artifacts and metrics
  E->>V: Request independent validation
  V->>V: Reproduce, audit leakage/costs/robustness/capacity
  alt Gates pass
    V-->>H: Validation report and limitations
    H->>R: Approve release stage (shadow/paper/limited live separately)
    R->>R: Create immutable signed/versioned release
    R-->>T: Publish eligible release event
  else Gates fail
    V-->>E: Record rejection and reason codes
    E-->>O: Return finding for research backlog
  end
```

## 6. Process restart and recovery

```mermaid
sequenceDiagram
  autonumber
  participant N as Runtime node
  participant DB as Operational DB/event store
  participant B as Broker/venue
  participant R as Reconciliation
  participant Q as Feed recovery
  participant S as Strategy workers
  N->>DB: Load committed checkpoints, config, pending outbox and risk reservations
  N->>B: Reconnect and authenticate using scoped secret
  N->>B: Query open orders, executions, positions, balances and margin
  B-->>R: Authoritative broker/account state
  DB-->>R: Internal OMS, ledger and portfolio state
  R->>R: Compare external and internal state; create break cases
  alt Reconciled and risk healthy
    R-->>N: Enable controlled trading state
    N->>Q: Reconnect market data and repair sequences/gaps
    Q-->>N: Feed quality restored
    N->>S: Restore state and replay from checkpoint idempotently
    S-->>N: Warmup complete / ready
  else Mismatch or uncertainty
    R-->>N: Keep affected account/instrument blocked
    N->>N: Alert operator and preserve evidence
  end
```

## 7. End-to-end trace IDs

For one hypothetical trade, the platform should be able to traverse:

`market_event_id → feature_snapshot_id → strategy_decision_id → target_id → allocation_decision_id → risk_decision_id → reservation_id → parent_order_id → child_order_id → client_order_id → broker_order_id → execution_id → ledger_transaction_id → reconciliation_case_id (if any)`.

The same correlation/causation IDs should appear in structured logs, traces, audit events and user-facing diagnostics. Not every event has every ID; preserve links rather than making them mandatory where they do not apply.
