# Bayan Applied NLP — My Project

# معالجة اللغات الطبيعية التطبيقية — مشروع بيان

**Student:** Abdulmajeed Mansour Bin Mosained · **GitHub:** [AbdulmajeedAlmosained](https://github.com/AbdulmajeedAlmosained)
**Program:** SDAIA Academy · `SDA-AIE-211` · Level: Specialist · #SDAIAAcademy
**Environment:** Google Colab (Free) + GitHub — no paid tools used
**Course trainer:** Meaad Al-Marri — credit for the course design and materials goes to her; the runs and evidence in this repo are mine. Official program org: https://github.com/SDAIAAcademy

---

## What this project is

**Bayan** is a bilingual (Arabic/English) NLP system I built over a 4-day applied course. It takes text from the Saudi government-services domain and pushes it through the full modern NLP pipeline:

1. **Text processing & tokenization** — cleaning, PII masking, Unicode handling for Arabic, and comparing tokenizers (local WordPiece vs multilingual BERT).
2. **Attention & transformers** — implementing scaled dot-product attention from scratch in NumPy, then inspecting real attention weights inside a pretrained model.
3. **Text classification** — fine-tuning DistilBERT on a bilingual topic-classification task, compared honestly against a TF-IDF baseline.
4. **NER & extractive QA** — token classification with BIO alignment and strict entity-boundary scoring, plus span extraction with an honest "no answer in context" option.
5. **Arabic NLP** — a documented normalization profile built with CAMeL Tools (diacritics, tatweel, alef folding), verified against golden test cases.
6. **Semantic search** — sentence embeddings + FAISS index over a bilingual case-resolution corpus, with a cross-encoder reranker and a measured latency/quality trade-off.
7. **Evaluation & error analysis** — bootstrap confidence intervals, sliced metrics, manual error taxonomy, and ranked fixes instead of vague claims.
8. **Optimization & serving** — ONNX export + dynamic INT8 quantization, measured against a written performance budget, wrapped in a tested FastAPI service with canary checks.

Every result in this repo is labeled what it is: `MEASURED_SMOKE` on small synthetic course data — not a production-quality claim.

## How the repo is organized

I work in **one merged notebook** that contains all nine course notebooks as sections, run top-to-bottom in a single Colab session:

- [Bayan_nlp_Abdulmajeed_Mansour_Bin_Mosained.ipynb](Bayan_nlp_Abdulmajeed_Mansour_Bin_Mosained.ipynb) — the full run: Day 1 → Day 4 in one file · [Open in Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/Bayan_nlp_Abdulmajeed_Mansour_Bin_Mosained.ipynb)

The nine official notebooks also live in [`notebooks/`](notebooks) (executed, with outputs), each openable in Colab from my repo:

| # | Notebook | Core check | Open in Colab (my repo) |
| --- | --- | --- | --- |
| 00 | [runtime_doctor](notebooks/00_runtime_doctor.ipynb) | `BAYAN_ENV_READY = True` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/00_runtime_doctor.ipynb) |
| 01 | [text_processing_tokenization](notebooks/01_text_processing_tokenization.ipynb) | `DAY1_NOTEBOOK1_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/01_text_processing_tokenization.ipynb) |
| 02 | [attention_transformers](notebooks/02_attention_transformers.ipynb) | `DAY1_NOTEBOOK2_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/02_attention_transformers.ipynb) |
| 03 | [text_classification](notebooks/03_text_classification.ipynb) | `DAY2_NOTEBOOK3_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/03_text_classification.ipynb) |
| 04 | [ner_and_qa](notebooks/04_ner_and_qa.ipynb) | `DAY2_NOTEBOOK4_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/04_ner_and_qa.ipynb) |
| 05 | [arabic_nlp](notebooks/05_arabic_nlp.ipynb) | `DAY3_NOTEBOOK5_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/05_arabic_nlp.ipynb) |
| 06 | [semantic_search](notebooks/06_semantic_search.ipynb) | `DAY3_NOTEBOOK6_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/06_semantic_search.ipynb) |
| 07 | [evaluation_error_analysis](notebooks/07_evaluation_error_analysis.ipynb) | `DAY3_NOTEBOOK7_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/07_evaluation_error_analysis.ipynb) |
| 08 | [optimization_serving](notebooks/08_optimization_serving.ipynb) | `DAY4_NOTEBOOK8_CORE=PASS` | [Colab](https://colab.research.google.com/github/AbdulmajeedAlmosained/bayan-nlp-AbdulmajeedBinMosend/blob/main/notebooks/08_optimization_serving.ipynb) |

Everything else (`src/`, `tests/`, `scripts/`, reports, and the required documents) comes from the official student starter package and is filled in by me.

## Results snapshot (Gate D, PROJECT_ARTIFACT)

| Candidate | p95 (ms) | Throughput (items/s) | Quality tax | Decision |
| --- | --- | --- | --- | --- |
| PyTorch FP32 (reference) | 234.0 | 35.5 | — | — |
| ONNX FP32 | 242.4 | 46.2 | 0.000 | ✅ adopted |
| ONNX dynamic INT8 | 167.7 | 60.0 | 0.565 | ❌ rejected (quality) |

Full detail in [`BENCHMARKS.md`](BENCHMARKS.md) and `reports/benchmark_results.json`. Scope: MEASURED_SMOKE on the course's small bilingual fixture.

## Reproduce my run

1. Open the merged notebook in Google Colab (link above).
2. **Runtime → Restart session and run all** (cells must run in order; a failed cell is never skipped).
3. Each section ends with its own check — look for `DAYx_NOTEBOOKx_CORE=PASS`.
4. Model weights and ONNX artifacts stay in `/content` (Colab) and are **not** committed — only their SHA-256 hashes, recorded in `reports/`.

Note: on a fresh Colab runtime I reinstall two inconsistent-by-default packages first —
`%pip install --force-reinstall --no-cache-dir --quiet scikit-learn==1.9.0` and
`%pip install --upgrade --quiet numpy` — then run all.

## My contribution

Everything measured in this repository is my own work: I ran all nine notebooks as one merged
notebook in my own Colab sessions (seed 42), produced every JSON/CSV report in `reports/` and the
root, wrote the ten required documents in my own words, and completed Gate D (re-running notebook 08
in `PROJECT_MODE` on my own fine-tuned checkpoint with my own written performance budget). Specific
evidence per claim: the executed notebooks carry the `DAYx_CORE=PASS` outputs, and each document
links to the files that back its statements.

## Evidence & reports

Generated by my runs and committed here:

- `day2_classification_metrics.json`, `day2_ner_qa_metrics.json`, `bayan_arabic_profile.json`, `runtime_report.json`
- `reports/` — model comparison, search manifest, retrieval metrics (incl. the reranker trade-off extension), slice report, error taxonomy, benchmark results, service smoke, validation report

## Honesty & privacy

- All data is the course's synthetic bilingual fixture — no real user data.
- PII patterns (emails, Saudi mobile numbers) are masked inside the pipeline.
- Small datasets mean wide confidence intervals; I report them instead of hiding them.

## Attribution & assistance

- Program: SDA-AIE-211, SDAIA Academy #SDAIAAcademy · https://github.com/SDAIAAcademy
- Trainer / course materials: Meaad Al-Marri — https://github.com/almiyead-rgb/bayan-applied-nlp-course
- All notebook runs, metrics and reports in this repository were produced by me in my own Google Colab sessions (seed 42).
- Assistance disclosure: I used AI assistance (Kimi/ChatGPT) for environment debugging (package version fixes), splitting my merged notebook into the nine course notebooks, and drafting documentation text. Every execution, number, and artifact here is from my own runs.

## Submission

Final tag: `submission-v1.0` · Validator: `BAYAN_SUBMISSION_VALIDATOR=PASS` (see `reports/submission_validation.json`)

---

© Meaad Al-Marri. Third-party libraries, datasets, references, and SDAIA Academy identity assets retain their respective rights and usage terms.
