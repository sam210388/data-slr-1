# Data for “What Do Employee Performance Prediction Models Actually Predict? A Systematic Review of Label Validity, Data Provenance, and Methodological Rigor (2021–2026)”

This repository holds the coding data behind the systematic review. Every percentage in the paper can be recomputed from `data/coding_v11.csv`.

## Files

| File | Content |
|---|---|
| `data/studies.csv` | The 151 selected studies: title, first author, year, DOI, and whether they were coded from the full text (139) or are available only as an introduction (12, excluded from all percentages). |
| `data/coding_v11.csv` | The final codes (version 11) for the 139 full-text studies on 13 variables in four analysis dimensions, with the supporting evidence for each code. |
| `data/blind_recoding_sample.csv` | The reliability check: 20 studies × 13 variables, with the first-pass code, the blind re-code, the adjudicated code, and the verdict. |
| `data/changes_v10_to_v11.csv` | The 109 codes changed after adjudication, after the re-check of error patterns, and after the evidence audits, each with a verbatim quotation and page. |
| `data/evidence_audit_sample.csv` | A random sample of 37 non-default codes (up to 3 per field, fixed seed), each checked against the full text by the first author in the review application: the verdict, the code proposed when it did not match, the code after adjudication, the time of the decision, and the hash of its entry in the append-only, hash-chained decision log. |
| `CODEBOOK.md` | Definitions and decision rules for every variable and code. |

## Reproducing the main figures

Each figure in Table I of the paper is a count of a code in `coding_v11.csv` divided by 139. For example:

```python
import pandas as pd
c = pd.read_csv("data/coding_v11.csv")
(c["label_leakage_risk"] == "yes").sum()          # 52 (37.4%)
(c["label_explanation"] == "explained").sum()      # 19 (13.7%)
flags = ["internal_inconsistency", "label_leakage_risk", "accuracy_ge_0_99_without_discussion",
         "bias_free_claim_without_measure", "superiority_claim_without_numbers"]
any_flag = (c["internal_inconsistency"] == "clear") | (c[flags[1:]] == "yes").any(axis=1)
any_flag.sum()                                     # 120 (86.3%)
```

## Notes

- **Evidence columns.** These contain verbatim quotations from the studies. Annotations that begin with `v9:`, `v11` or `v11 bukti` are coder notes added during re-verification, some written in Indonesian. Several quotations are joined with `||`.
- **Full texts are not included.** Copyright stays with the publishers; use the DOI links in `studies.csv`.
- **Random evidence audit.** The first author checked 37 randomly drawn codes against the full text: 34 matched (91.9%; 95% Wilson interval 79–97%). Of the 3 that did not, 2 were corrected after adjudication (study 162, own label reliability; study 204, data source) and 1 was upheld (study 253, label explanation: the quoted definition comes from the literature review, not from the study's own data).
- **Borderline cases.** Of the 96 disagreements in the blind re-coding sample, 14 could be read either way under the codebook. The first author decided each of them after re-reading the full text; the decisions are recorded in the review application's append-only log, and the verdict column of `blind_recoding_sample.csv` marks them as "borderline, decided by first author".
- **Evidence audit.** For all 139 studies, every code other than the default was checked automatically against the paper text: each has at least one quotation found in the text (exactly or approximately), or a transcription of the table or figure it rests on, marked `[isi tabel/gambar]`.
- **Studies first coded from publisher web pages.** Nine studies were coded from publisher web pages. The PDFs were obtained later for all nine, and their codes were re-checked and quoted from them.

## License

The data and documentation are released under the Creative Commons Attribution 4.0 International license (CC BY 4.0); see `LICENSE`.
