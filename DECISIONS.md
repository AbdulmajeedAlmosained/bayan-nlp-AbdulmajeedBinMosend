# Decision Log

## D1 — Tokenizer choice (Day 1)
- **Decision:** use the fast pretrained tokenizer from `bert-base-multilingual-cased` for the
  pipeline, after demonstrating the mechanics with a locally trained WordPiece tokenizer.
- **Why:** the local tokenizer proved the concepts (fertility, truncation, special tokens) on a
  tiny vocab, but the pretrained tokenizer is consistent with the fine-tuned model and handles
  Arabic + English in one vocabulary.
- **Evidence:** notebook 01 fertility/truncation metrics.

## D2 — Classification model (Day 2)
- **Decision:** fine-tune `distilbert-base-multilingual-cased` instead of a monolingual Arabic BERT.
- **Why:** the task is bilingual (ar + en); the multilingual checkpoint shares one representation
  across both languages. It also trains faster than the larger mBERT.
- **Baseline discipline:** every transformer result is reported next to the TF-IDF + LinearSVC
  baseline with the macro-F1 delta, never alone.

## D3 — NER evaluation protocol (Day 2)
- **Decision:** strict entity-boundary scoring (a partially correct span scores zero) and a
  rule-based `best_span` extractor with an explicit "no answer in context" branch for QA.
- **Why:** token-level accuracy hides boundary errors; strict spans keep the metric honest.

## D4 — Arabic normalization profile (Day 3)
- **Decision:** a named "search" profile (CAMeL Tools 1.6.0): Unicode NFC, strip tatweel and
  diacritics, fold alef variants and alef maksura, mask course PII patterns — while preserving
  the original display copy untouched.
- **Why:** retrieval queries and documents must match under spelling variation; the golden-case
  suite (`bayan_arabic_profile.json`) proves each rule.

## D5 — Retrieval stack (Day 3)
- **Decision:** bi-encoder (`paraphrase-multilingual-MiniLM-L12-v2`) + FAISS `IndexFlatIP` for
  candidate retrieval, then a cross-encoder (`mmarco-mMiniLMv2-L12-H384-v1`) reranker over 6
  candidates, with the no-answer threshold tuned on validation only.
- **Evidence:** `reports/retrieval_metrics.json` — MRR@3 before/after reranking, with latency
  (median and p95) recorded and warmup excluded.

## D6 — Serving artifact (Day 4 / Gate D)
- **Decision:** adopt the dynamic-INT8 ONNX artifact if it meets the written budget with
  acceptable quality tax; otherwise fall back to ONNX FP32, and only then to PyTorch.
- **Why:** INT8 gives the largest size/latency win; the decision is made by measured budget
  assessment, not by assumption. Rollback path: re-export from the recorded model source.
