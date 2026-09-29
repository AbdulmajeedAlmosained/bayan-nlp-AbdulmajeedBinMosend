# Benchmarks — Bayan (PROJECT_ARTIFACT)

> Numbers below come from re-running notebook 08 in `PROJECT_MODE` on my own fine-tuned
> DistilBERT classifier (Gate D). Full machine-readable detail: `reports/benchmark_results.json`.

## Workload
- 8 bilingual (ar/en) validation sentences, batch size 4, max_length 96, CPU
  (`colab-cpu`), ORT `CPUExecutionProvider`.
- Token lengths audited first: p50 = 11, p95 = 14.65, max = 15 tokens — no truncation.
- Measurement protocol: 5 warmup + 30 timed repetitions, p50/p95/p99 + throughput,
  process-RSS memory method.

## Budget (written before measurement)
| Metric | Budget |
|---|---|
| p95 latency | 500 ms |
| min throughput | 1.0 items/s |
| max quality tax | 0.05 macro-F1 |
| target device | colab-cpu |

## Results (measured on my own fine-tuned classifier)
| Candidate | p95 (ms) | Throughput (items/s) | Quality tax | Budget met? |
|---|---|---|---|---|
| PyTorch FP32 (reference) | 234.0 | 35.5 | — (reference) | — |
| ONNX FP32 | 242.4 | 46.2 | 0.000 | ✅ YES |
| ONNX dynamic INT8 | 167.7 | 60.0 | 0.565 | ❌ NO (quality tax 0.565 ≫ 0.05 budget; prediction agreement only 0.50) |

- Quality metric: macro-F1 on the full bilingual validation workload
  (baseline quality = 1.0 for the PyTorch reference).

## Decision
- **Selected for service: `onnx-fp32` — `ADOPT_ONNX_FP32`** (recorded in
  `reports/benchmark_results.json` as `PROJECT_BUDGET_DECISION`).
- INT8 was **rejected, not assumed**: it is 1.4× faster, but dynamic quantization
  destroyed quality (macro-F1 1.0 → 0.43, tax 0.565), so it failed the written
  quality budget. Adopting it anyway would have been a silent quality regression.
- Parity before adoption: ONNX FP32 max |Δlogit| ≈ 2.9e-6 (< 1e-3) and prediction
  agreement = 1.0 vs the PyTorch reference on the full workload.
- Rollback: re-export from the recorded model source; weights stay outside this repository.
- Honest scope: smoke-scale workload (8 sentences) and CPU runtime — not a production benchmark.
