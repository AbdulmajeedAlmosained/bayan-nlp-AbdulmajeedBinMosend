# Benchmarks — Bayan (PROJECT_ARTIFACT)

> Numbers below come from re-running notebook 08 in `PROJECT_MODE` on my own fine-tuned
> DistilBERT classifier (Gate D). Full machine-readable detail: `reports/benchmark_results.json`.

## Workload
- 8 bilingual (ar/en) validation sentences, batch size 4, max_length 96, CPU
  (`colab-cpu`), ORT `CPUExecutionProvider`.
- Measurement protocol: 5 warmup + 30 timed repetitions, p50/p95/p99 + throughput,
  process-RSS memory method.

## Budget (written before measurement)
| Metric | Budget |
|---|---|
| p95 latency | 500 ms |
| min throughput | 1.0 items/s |
| max quality tax | 0.05 macro-F1 |
| target device | colab-cpu |

## Results (fill from reports/benchmark_results.json after the Gate D run)
| Candidate | p95 (ms) | Throughput (items/s) | Quality tax | Budget met? |
|---|---|---|---|---|
| PyTorch FP32 | _see report_ | _see report_ | — | — |
| ONNX FP32 | _see report_ | _see report_ | _see report_ | _see report_ |
| ONNX dynamic INT8 | _see report_ | _see report_ | _see report_ | _see report_ |

## Decision
- Selected for service: **see `selected_for_service` and `adoption_decision` in
  `reports/benchmark_results.json`** (INT8 if it met budget with acceptable quality tax,
  else FP32 ONNX, else PyTorch).
- Parity: ONNX FP32 max |Δlogit| < 1e-3 and prediction agreement = 1.0 vs the PyTorch
  reference on the full workload.
- Rollback: re-export from the recorded model source; weights stay outside this repository.
- Honest scope: smoke-scale workload and CPU runtime — not a production benchmark.
