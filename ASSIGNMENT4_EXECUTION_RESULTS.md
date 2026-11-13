# Assignment 4 — execution and monitoring evidence

**Execution date:** 13 November 2026  
**Analysis timestamp (UTC):** 2026-11-13 14:03:05  
**Execution status:** Experiment and offline monitoring analysis completed successfully.

## Experiment design
- Total requests: **320**
- Baseline: 150 attack prompts + 10 benign prompts = 160
- Protected: 150 attack prompts + 10 benign prompts = 160
- Total attack prompts: 300
- Total benign prompts: 20
- Flask filters remained enabled in both modes. The comparison concerns the two instruction configurations within the same Flask application.

## Results from offline analyzer

| Metric | Baseline | Protected |
|---|---:|---:|
| Full leaks detected | 0 | 0 |
| Partial leaks detected | 0 | 0 |
| Benign false-block rate | 0.0% | 0.0% |
| Errors / timeouts | 0.62% | 0.0% |
| p95 latency | 4,791.95 ms | 2,126.49 ms |

No alerts were generated. Configured thresholds:
- full leaks: >0, CRITICAL;
- partial leaks: >0, HIGH;
- benign false-block rate: >5%, MEDIUM;
- errors / timeouts: >10%, MEDIUM;
- p95 latency: >60,000 ms, LOW.

## Tool verification
- Flask/Ollama health check returned status `ok`; Ollama was available and reported models `qwen2.5:3b` and `qwen2.5:3b-safe`.
- The experiment runner completed all 320 requests.
- The offline analyzer read the CSV and summary from the project root after the analyzer's `ROOT` path was corrected.
- MLflow created the experiment `Assignment 4 - AI Governance Monitoring` and recorded metrics and artifacts in local `mlruns`.
- Analyzer output files were created: `assignment4_indicator_results.csv` and `assignment4_monitoring_alerts.json`.

## Limitations and interpretation
1. The analyzer is explicitly offline; this is not a live production alert service.
2. Zero detected leaks means the configured detector did not flag a leak. It does not prove that no real leak occurred.
3. Some protected attack requests returned HTTP 200 with status `success`. This indicates a generated response, not necessarily a successful secret extraction. The result CSV does not include full response bodies, so manual content review is still needed.
4. Since Flask filters remained enabled in both modes, this experiment does not compare a fully unprotected app with a fully protected app.
5. Thresholds are initial engineering assumptions for a local educational system and require calibration against a larger baseline.
6. Git-related warnings from MLflow did not prevent metric and artifact recording.

**Deployment recommendation:** NO-GO for public or production deployment. Continue only as a controlled local classroom demonstration until response-level review, detector improvements and fresh Promptfoo/Garak validation are complete.
