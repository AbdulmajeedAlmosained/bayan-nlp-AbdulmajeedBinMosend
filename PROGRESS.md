# Progress Log

| Day | What I did | Evidence (commit / file) |
|---|---|---|
| Setup | Created the repo, uploaded the student starter package, completed STUDENT_PROFILE.md | initial commits on main |
| Day 1 | Ran notebooks 00-02 in one merged Colab session: environment doctor passed (`BAYAN_ENV_READY = True`), built the two-copy text-preprocessing pipeline with PII masking, trained a local WordPiece tokenizer, implemented scaled dot-product attention in NumPy, and inspected real attention weights from a pretrained multilingual model | `Bayan_nlp_Abdulmajeed_Mansour_Bin_Mosained.ipynb` (sections 00-02), `runtime_report.json` |
| Day 2 | Fine-tuned DistilBERT (mDeBERTa-family checkpoint `distilbert-base-multilingual-cased`) on the bilingual topic-classification fixture and compared it honestly against a TF-IDF + LinearSVC baseline; trained NER (BIO alignment, strict boundary scoring) and extractive QA with an explicit no-answer path | `day2_classification_metrics.json`, `day2_ner_qa_metrics.json` |
| Day 3 | Built the documented Arabic normalization profile with CAMeL Tools 1.6.0 (search profile: diacritics/tatweel removal, alef folding, PII masking) verified against golden cases; built the bilingual FAISS semantic-search index with a tuned no-answer threshold and a measured cross-encoder reranker trade-off; ran the full evaluation notebook (bootstrap CIs, slices, manual error taxonomy, ranked fixes) | `bayan_arabic_profile.json`, `reports/` |
| Day 4 | Exported the model to ONNX (FP32 + dynamic INT8), verified numerical parity and prediction agreement, benchmarked against a written performance budget, and served the selected artifact through a FastAPI app with startup canaries and contract tests | `reports/benchmark_results.json`, `reports/service_smoke.json` |
| Gate D | Re-ran the optimization notebook on my own fine-tuned classifier as `PROJECT_ARTIFACT` (see PROJECT_SUMMARY.json) | final notebook version + updated `reports/` |

Known issue handled: Colab's preinstalled scikit-learn was internally inconsistent, so I
force-reinstall `scikit-learn==1.9.0` as the first cell before every full run.
