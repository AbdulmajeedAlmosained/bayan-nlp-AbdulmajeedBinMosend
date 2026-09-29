# Evaluation Report

**Scope:** all numbers below are `MEASURED_SMOKE` results on the course's small synthetic
bilingual fixture. They prove the pipeline works end-to-end; they are **not** estimates of
production quality.

## Classification (Day 2)
- Transformer (fine-tuned DistilBERT) vs TF-IDF + LinearSVC baseline, macro-F1 on validation,
  with the delta reported explicitly in `day2_classification_metrics.json`.
- Split contract enforced: group-level isolation between train/validation/test, zero group
  overlap, every label present in every split.

## NER (Day 2)
- Strict span-level precision/recall/F1 in `day2_ner_qa_metrics.json`; a boundary test where a
  partially-correct span scores F1 = 0.0 by design.
- Sub-word alignment with `-100` on continuation tokens so special/padding tokens never
  contribute to loss or metrics.

## Extractive QA (Day 2)
- Offset-to-token alignment verified on the training fixture; rule-based `best_span` with a
  null margin, unit-tested on a valid Arabic span ("الرياض") and on an honest no-answer case
  (`reason: no_answer_in_context`).

## Arabic dialect robustness (Day 3)
- Frozen Gulf-only test slice; two checkpoints compared (multilingual DistilBERT vs CAMeLBERT-da)
  in `reports/arabic_model_comparison.json` — validation macro-F1 and Gulf macro-F1 reported
  per model, single seed, explicitly labeled descriptive.

## Semantic search (Day 3)
- recall@3 and MRR@3 on the answerable test queries; per-slice breakdown by language and
  retrieval mode, every slice flagged SMALL_SLICE where n < 10.
- No-answer threshold tuned on validation only, then frozen and applied to test
  (`reports/retrieval_metrics.json`).
- Reranker measured with warmup excluded; median and p95 latency recorded on CPU.

## Error analysis (Day 3)
- 1000-resample bootstrap CIs around macro-F1 for two prediction sets; paired bootstrap for the
  directional claim (interval included zero for this fixture, so no superiority claim is made).
- Six behavioral cases and a manual 8-error taxonomy (dialect_gap, class_confusion,
  hard_or_ambiguous) in `reports/day3_error_taxonomy.csv`, plus three ranked fixes with
  acceptance tests in `reports/day3_evaluation_fixture.json`.

## Conclusion
The system meets its stated smoke goals; the limiting factor is data size, not method. The
ranked fixes (more Gulf coverage, contrastive examples, abstention on short ambiguous requests)
are the honest next steps.
