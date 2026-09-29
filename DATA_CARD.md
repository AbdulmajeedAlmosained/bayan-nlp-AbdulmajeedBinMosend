# Data Card

## Sources
All datasets are the course's synthetic bilingual (Arabic/English) teaching fixtures, fetched
at run time from the public course repository (with an embedded fallback copy inside each
notebook). No external, private, or real user data is used.

## Fixtures
| File | Size | Content |
|---|---|---|
| `bayan_day2_classification.csv` | 24 rows, grouped | topic classification (4 topics), train/validation/test with group isolation |
| `bayan_day2_ner.jsonl` | 10 sequences | BIO NER (SERVICE, LOCATION, DATE, REF_NUM, ORG) |
| `bayan_day2_qa.json` | 10 contexts | extractive QA incl. intentional no-answer test items |
| `bayan_day3_arabic.csv` | 20 rows | MSA / Gulf / Arabizi variants for the normalization profile |
| `bayan_day3_cases.csv` + `bayan_day3_queries.jsonl` | 24 cases, 18 queries | semantic-search corpus and ranked queries (mono/cross-lingual/no-answer) |
| `bayan_day3_predictions.csv` | 36 rows | paired predictions for evaluation/bootstrap analysis |

## Processing
- Text passes through the documented preprocessing profile (Arabic: NFC, strip tatweel and
  diacritics, fold alef variants; both languages: whitespace normalization, course PII masking).
- Raw display copies are preserved; only the model-facing copy is normalized.
- The search corpus is hashed (SHA-256) and recorded in `reports/search_manifest.json`.

## Known limitations
- Tiny, synthetic, template-generated: wide confidence intervals; slice sizes flagged
  SMALL_SLICE; results are labeled MEASURED_SMOKE and must not be quoted as production quality.
- Arabizi rows are routed by a transparent heuristic, not by a dialect classifier.

## Privacy
No personal data. PII patterns appearing in examples (learner@example.org, 05xxxxxxxx) are
synthetic and are masked by the pipeline regardless.
