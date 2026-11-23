# Experiment register — include negative results

| Experiment ID | Hypothesis ID | Data release | Code commit | Search trials | Costs | OOS period | Outcome | Known issues |
|---|---|---|---|---|---|---|---|---|
| SAMPLE-001 | synthetic only | synthetic-0 | untracked example | 1 | sample costs | not applicable | example—not evaluated | never interpret as market evidence |

## Requirements

- Allocate an experiment ID **before** running an extensive parameter search.
- Record every tested configuration or reproducible search specification.
- Store one untouched holdout period after choosing features/parameters.
- Link figures to exact data, code and metrics, not filenames alone.
- Archive rejected experiments with reasoning; do not delete trials to improve apparent performance.
- Make corrected dataset releases invalidate relevant experiment results via lineage rather than mutating previous results.
