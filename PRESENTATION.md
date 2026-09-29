# Presentation Plan (10 slides, 5-7 minutes)

1. **Title** — Bayan: bilingual NLP over Saudi government-services text. Name + repo link.
2. **Problem & data** — ar/en user requests across 4 service topics; small synthetic fixture,
   honest about its size; show one Arabic + one English example.
3. **Text → numbers (Day 1)** — two-copy preprocessing contract, PII masking demo before/after,
   why tokenizer choice matters (fertility + truncation numbers).
4. **Attention in 60 seconds** — my NumPy scaled dot-product attention; one real attention
   heatmap from the pretrained model; the 1/√d_k scaling effect on entropy.
5. **Classification (Day 2)** — DistilBERT fine-tune vs TF-IDF baseline; macro-F1 delta; split
   isolation (why group leakage would inflate scores).
6. **NER & QA (Day 2)** — BIO alignment with -100; strict boundary F1; the honest no-answer QA
   demo (question whose answer is not in the context).
7. **Arabic day (Day 3)** — the CAMeL "search" profile with golden-case table; Gulf frozen-slice
   comparison of two checkpoints; live demo: messy Gulf input → clean retrieval.
8. **Search + reranker trade-off (Day 3)** — recall@3/MRR@3 before vs after reranking, with
   median/p95 latency; why we adopted or rejected the reranker based on measurement.
9. **Optimization & serving (Day 4)** — ONNX FP32 → INT8, parity checks (prediction agreement =
   100%), budget table, FastAPI `/health` + `/v1/classify` with canaries and contract tests.
10. **Honesty & next steps** — every number is MEASURED_SMOKE; error taxonomy top-3 fixes;
    one measured extension idea. Closing with the repo link and the validator PASS line.

**Demo fallback:** all notebooks are committed with outputs, so every number can be shown
from the repo even if live inference is not possible in the room.
