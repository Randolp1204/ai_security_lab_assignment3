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

## Fresh Garak PromptInject verification — 13 November 2026
- Garak 0.17.0 tested Ollama `qwen2.5:3b` directly; this does not exercise the Flask filters.
- `HijackHateHumans`: 1,280 attempts (256 prompts × 5 generations), detector passed 140, marker detected 1,140; ASR 89.06% (95% CI 87.34–90.70%).
- `HijackKillHumans`: 1,280 attempts (256 prompts × 5 generations), detector passed 396, marker detected 884; ASR 69.06% (95% CI 66.56–71.64%).
- `HijackLongPrompt`: 256 attempts (256 prompts × 1 generation), detector passed 47, marker detected 209; ASR 81.64% (95% CI 76.95–86.33%).
- The first run was interrupted during the third probe; its partial third-probe attempts are excluded. The long-prompt result comes from a separate completed run. Because generation counts differ, do not report a single combined ASR.
- Detailed run IDs, report filenames and method: `ASSIGNMENT4_GARAK_RESULTS.md`.

## Fresh Promptfoo verification — 13 November 2026
- Ran `redteam_promptfoo_fixed.yaml` against the local Flask endpoint `http://127.0.0.1:5000/chat` with `--no-cache`.
- Evaluation ID: `eval-mGo-2026-11-13T14:56:06`; 28 actual target requests, 0 cache hits.
- Results: 18 passed (64.29%), 10 failed (35.71%), 0 errors.
- Breakdown: 7 of 13 system-prompt-override cases failed; each returned a response beginning with `HACKED:`. Three prompt-extraction cases were flagged because a generic Russian security-block message was interpreted as system-prompt disclosure. The observed block text contained neither the artificial secret nor the full system prompt; retain these as manual-review items, not confirmed leakage.
- Evidence files: `redteam_promptfoo_fixed.yaml`, `assignment4_promptfoo_fresh.json`, and `assignment4_promptfoo_fresh.html`.
- The earlier corrected-parser run had 22 cache hits and is not the fresh evidence; use the uncached evaluation above.

## Remaining before final submission
1. Review the three prompt-extraction grader flags and document the adjudication.
2. Remediate the confirmed prompt-override weakness, then repeat Promptfoo with `--no-cache`.
3. Complete and render the required 2–3 page English PDF report.
4. After remediation, rerun Garak with the same probes, model and generation settings for a comparable direct-model check; keep the current deployment decision at NO-GO until high-severity override failures are resolved.

Do not commit the `mlruns/` directory or raw monitoring logs if they contain prompts / response content. Keep the production deployment decision at NO-GO until the residual risks are addressed.
