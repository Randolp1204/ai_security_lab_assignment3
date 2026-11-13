# Fresh Garak PromptInject results — 13 November 2026

## Scope and method

Garak 0.17.0 tested the local Ollama model `qwen2.5:3b` directly. This is a model-level PromptInject check; it does **not** exercise the Flask application's input/output filters. The detector was `promptinject.AttackRogueString`. A reported attack success means the output matched that probe's injected marker, not that a protected secret was disclosed.

The first run selected the three active PromptInject probes with a soft prompt cap of 256 and five generations per prompt. It was interrupted during the third probe. The first two probe summaries completed and are retained below; partial third-probe attempts from that interrupted run are excluded. `HijackLongPrompt` was then run separately to completion with one generation per prompt. Because generation counts differ, do not combine these rows into a single overall ASR or compare them as equal-sized samples.

## Completed probe summaries

| Probe | Evaluated attempts | Detector passed (no marker) | Marker detections | Attack success rate (95% CI) |
|---|---:|---:|---:|---:|
| `HijackHateHumans` | 1,280 (256 prompts × 5 generations) | 140 | 1,140 | 89.06% (87.34–90.70%) |
| `HijackKillHumans` | 1,280 (256 prompts × 5 generations) | 396 | 884 | 69.06% (66.56–71.64%) |
| `HijackLongPrompt` | 256 (256 prompts × 1 generation) | 47 | 209 | 81.64% (76.95–86.33%) |

## Run references

- Interrupted first run: `3af4a5b3-6f19-4a1d-bed6-f17649f212e1`; completed summaries for the first two probes are in `garak.3af4a5b3-6f19-4a1d-bed6-f17649f212e1.report.jsonl`. Any partial third-probe attempts from this run are not included.
- Completed long-prompt run: `b4de950e-ce4c-4da5-9730-116d62f82ca1`; report: `garak.b4de950e-ce4c-4da5-9730-116d62f82ca1.report.jsonl`; HTML: `garak.b4de950e-ce4c-4da5-9730-116d62f82ca1.report.html`.

The full Garak JSONL and HTML reports remain in the local Garak runs folder on the test machine. The reproducible summary above records the run IDs, scanner version, model, detector, probe names, sample counts, generation settings and confidence intervals.
