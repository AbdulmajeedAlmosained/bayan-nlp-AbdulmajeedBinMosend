# Model Card — Bayan NLP

## Primary artifact (Gate D)
- Base checkpoint: `distilbert/distilbert-base-multilingual-cased` (pretrained, weights downloaded
  from Hugging Face at run time; no API key required).
- Adaptation: sequence-classification head fine-tuned on the course's bilingual ar/en topic
  fixture (4 topics; training mode recorded per device: full fine-tune on GPU, last-block + head
  on CPU).
- Output: one of {digital_service, permit, health, transport} with softmax confidence.

## Supporting models used in the pipeline
- `google-bert/bert-base-multilingual-cased` — tokenizer reference and parameter audit.
- `CAMeL-Lab/bert-base-arabic-camelbert-da` — Arabic comparison candidate (Day 3).
- `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2` — retrieval bi-encoder.
- `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1` — reranker.

## Intended use
Educational bilingual text classification / retrieval over Saudi government-services topics.
Not for production decisions; smoke-scale training data only.

## Evaluation
See EVALUATION_REPORT.md and the JSON artifacts in `reports/` — every number is labeled
`MEASURED_SMOKE` with its limitations listed inline.

## Privacy & safety
- Synthetic course data only; no real user data anywhere in the repo.
- PII patterns (emails, Saudi mobile numbers) are masked to `<EMAIL>` / `<PHONE>` inside the
  model-facing text; the raw display copy is preserved separately and never leaves the pipeline.
- No secrets, tokens, or credentials are stored in this repository.

## Rollback
Re-export ONNX from the recorded model source; model weights are kept outside GitHub
(hashes recorded in `reports/benchmark_results.json`).
