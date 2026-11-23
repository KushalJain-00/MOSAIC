# MOSAIC Documentation Hub — additive expansion v0.1

This directory is an **additive documentation pack**, not a replacement for the existing `docs/architecture/00`–`22` chapters. Do not overwrite your existing architecture files. Merge the new folders into `MOSAIC/docs/` and compare any same-named indexes before copying.

## Where to start

1. `learning-material/README.md`: study map and Socratic protocol.
2. `learning-material/00-diagnostic.md`: initial placement, one question at a time.
3. `learning-material/quant-foundations/`: probability, statistics, time series.
4. `data-platform/README.md`: Indian-market data V1 operational design.
5. `strategy-research/README.md`: research process from hypothesis to evaluation.
6. `engineering/README.md`: code, migration, testing, recovery and resource rules.
7. `assessment/README.md`: mixed capstones and rubrics.
8. `governance/documentation-lifecycle.md`: keep the documentation current as MOSAIC evolves.

## New folders and ownership

| Folder | Owns | Doesn't own |
|---|---|---|
| `learning-material/` | Principles, formulas, guided derivations, learning skill map | Production architecture decisions |
| `data-platform/` | V1 acquisition/storage contract, operational acceptance | General quant pedagogy |
| `strategy-research/` | Hypothesis templates, test methodology, strategy mechanics | Live strategy authorization |
| `engineering/` | Tooling, migrations, interfaces, tests, CI, deployment practices | Exchange market mechanics |
| `assessment/` | Diagnostic and capstone evaluations, separate evaluator notes | Uncontrolled answer leaks |
| `governance/` | Documentation ownership, statuses, versioning, review | Code or market data |
| `reference/` | Formula index, terminology, external reference links | Unsourced implementation claims |

**Existing folders to preserve:** `architecture/`, `roadmap/`, and `docs/learning-material/` if already present. This pack does not assume your screenshot shows every file on disk.

## Reading convention

- `> **Assumption**`: not verified against a provider or legal source.
- `> **Invariant**`: must hold in production code.
- `> **Open question**`: must be resolved before implementing dependent features.
- Mermaid fences render flowcharts and dependency graphs in supported Markdown viewers (including VS Code preview with Mermaid support).
- PNG figures under `learning-material/assets/` are **illustrative synthetic data**, not live market statistics.
- Documents labelled **future** describe target architecture, not claims that the corresponding component exists.

## Implementation principle

The full MOSAIC architecture supports multiple markets, but the current vertical slice is a **local-first Indian EOD data platform** using permitted inputs; deeper derivatives, tick/order-book data and trading are later capabilities. Avoid turning conceptual diagrams into unmeasured microservices.
