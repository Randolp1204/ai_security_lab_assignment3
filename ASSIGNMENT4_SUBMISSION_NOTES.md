# Assignment 4 — submission notes

Draft artifacts added to this branch:
- `ASSIGNMENT4_REPORT_DRAFT.md`
- `monitoring_indicators.csv`
- `oversight_workflow.mmd`

## Important evidence limitations
1. Garak figures are historical results from prior runs, not a new run performed while preparing these documents.
2. Promptfoo figures are historical recorded results. The repository contains `redteam.yaml`, but not a verified current Promptfoo result artifact.
3. `results.csv` has an older schema than the current `run_experiment.py`. It must not be presented as a valid controlled comparison proving the protected flag improved results.
4. Re-run the current experiment and Promptfoo locally and attach fresh output files before final submission.
5. Current recommendation is NO-GO for production; local classroom demonstration only.

## Local rerun checklist
- Start Ollama and confirm both `qwen2.5:3b` and `qwen2.5:3b-safe` are available.
- Start the Flask app and verify `GET /health`.
- Run `python run_experiment.py` and preserve the new `results.csv` plus console summary.
- Run Promptfoo against the existing generated suite with `promptfoo redteam eval -c redteam.yaml`; save the report.
- Preserve the exact Garak command, model, probe names, counts, and report.
- Update the report only with fresh, reproducible values.
