# Parquet layout, pruning and analytical query plans

> **Track:** `data-engineering` · **Lesson:** 3 · **Target:** independent reasoning (4/5)  
> **Prerequisite:** read the track index and complete prior diagnostic questions.  
> **Outcome:** Choose file layouts that avoid tiny-file explosion and unnecessary full scans.

## Socratic opening — answer before reading

1. Why not create one file per contract per day?
2. Can a poorly sorted Parquet file weaken predicate pushdown?
3. Which is more important: predictable scan cost or visually intuitive folder names?

**Tutor protocol:** ask one question at a time in a live session. The tutor should not reveal the worked solution until the learner has attempted the problem and been probed on the reasoning. These questions are a study bank, not evidence of mastery.

## Core reasoning

Partitioning trades metadata lookup cost for pruning. A date/venue/segment partition may work for broad EOD scans; excessive instrument/day partitioning creates many tiny files. Sorting rows and using Parquet row-group statistics enables filtering. DuckDB predicate and projection pushdown limit disk reads when file metadata supports it.

## Mathematical or engineering model

Illustrative estimate: $N_{files}=N_{instruments}\times N_{days}$ for a one-file-per-instrument-day layout. With 2,000 instruments and 250 days/year, that is **500,000 files/year**, before segmenting by timeframe. A grouping scheme can reduce file count while preserving filterability.

## Worked example (read after attempting)

A researcher wants one contract in March 2024. The manifest narrows candidate files to March F&O partitions, then DuckDB prunes unwanted columns and row groups. A benchmark should measure bytes scanned and open-file overhead, not only wall-clock time.

## Visual model

```mermaid
flowchart LR
 Q[Instrument + date + version] --> CAT[Manifest filter]
 CAT --> PART[Relevant partitions]
 PART --> PARQ[Parquet row-group pruning]
 PARQ --> OUT[Projected columns and rows]
```

## Failure modes and misconceptions

Identify the assumptions and find a counterexample before adopting the model. In MOSAIC, keep raw observations distinct from modelling assumptions.

## Build or paper exercise

Design two candidate partitions for equity EOD and F&O EOD. Calculate approximate file counts and propose a query benchmark over synthetic data.

## Verification and cross-connections

Verify dimensional units, edge cases, reproducibility and whether each input was actually observable at decision time.

## Evidence and references

[DuckDB Parquet guide](https://duckdb.org/docs/stable/data/parquet/overview) · [DuckDB Parquet tips](https://duckdb.org/docs/current/data/parquet/tips)

## Mastery gate

To claim **4/5**, solve two independent variants, including one without hints after a delay. To claim **5/5**, explain a counterexample, show an edge case and teach the model clearly. Record evidence in `docs/learning-material/MASTERY-TRACKER.md`.
