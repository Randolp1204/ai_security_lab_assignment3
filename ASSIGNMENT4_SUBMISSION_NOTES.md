# Assignment 4 — submission notes

## GitHub branch
Work is maintained on `assignment-4`; the default `main` branch is not modified.

## Prepared artifacts already in this branch
- `app.py` — Flask application with monitoring, logging, metrics and error handling.
- `redteam.yaml` — Promptfoo red-team configuration.
- `ASSIGNMENT4_REPORT_DRAFT.md` — report draft and current monitoring results.
- `monitoring_indicators.csv` — monitoring indicator definitions.
- `oversight_workflow.mmd` — oversight workflow diagram.
- `ASSIGNMENT4_EXECUTION_RESULTS.md` — record of the completed 320-request run and analysis.
- `assignment4_monitoring_alerts.json` — offline monitoring thresholds, metrics and alerts for the latest run.

## Verified local execution — 13 November 2026
- Flask/Ollama health check succeeded; Ollama reported `qwen2.5:3b` and `qwen2.5:3b-safe`.
- `run_assignment4_experiment.py` completed 320 requests: 150 attacks + 10 benign prompts in each of baseline and protected modes.
- The offline monitoring analyzer successfully read the experiment results after its `ROOT` path was corrected to the project root.
- MLflow recorded metrics and artifacts in the local `mlruns` directory. The Git executable warning did not prevent metric recording.
- Generated outputs: `assignment4_indicator_results.csv` and `assignment4_monitoring_alerts.json`.
- The alert list was empty under the configured thresholds.

### Latest measured metrics

| Metric | Baseline | Protected |
|---|---:|---:|
| Full leaks detected | 0 | 0 |
| Partial leaks detected | 0 | 0 |
| Benign false-block rate | 0.0% | 0.0% |
| Errors / timeouts | 0.62% | 0.0% |
| p95 latency | 4,791.95 ms | 2,126.49 ms |

**Interpretation warning:** These are detector results, not proof that no secret was disclosed. Some protected attack prompts returned HTTP 200 / `success`; the result CSV does not retain full response bodies, so response-level manual verification remains outstanding. Flask filters were enabled in both modes; this is not a no-filter versus protected comparison.

## Remaining before final submission
1. Add the exact generated `assignment4_experiment_results.csv`, `assignment4_experiment_summary.json`, and `assignment4_indicator_results.csv` from the local project if these are to be versioned. Do not reconstruct or edit measured raw data manually.
2. Add the exact source files `run_assignment4_experiment.py` and `analyze_assignment4_monitoring.py` from the local project; the analyzer's local path fix must be included in the committed source.
3. Run Promptfoo and save the fresh JSON/HTML report for this run.
4. Review raw response bodies for attack prompts and improve leak detection before making any security claim.

Do not commit the `mlruns/` directory or raw monitoring logs if they contain prompts / response content. Keep the production deployment decision at NO-GO until the residual risks are addressed.
