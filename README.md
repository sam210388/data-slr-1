# Data for “What Do Employee Performance Prediction Models Actually Predict? A Systematic Review of Label Validity, Data Provenance, and Methodological Rigor (2021–2026)”

This repository holds the coding data behind the systematic review. Every percentage in the paper can be recomputed from `data/coding_v11.csv`.

## Files

| File | Content |
|---|---|
| `data/studies.csv` | The 151 selected studies: title, first author, year, DOI, and whether they were coded from the full text (139) or are available only as an introduction (12, excluded from all percentages). |
| `data/coding_v11.csv` | The final codes (version 11) for the 139 full-text studies on 13 variables in four analysis dimensions, with the supporting evidence for each code. |
| `data/blind_recoding_sample.csv` | The reliability check: 20 studies × 13 variables, with the first-pass code, the blind re-code, the adjudicated code, and the verdict. |
| `data/changes_v10_to_v11.csv` | The 96 codes changed after adjudication and after the re-check of error patterns, each with a verbatim quotation and page. |
| `CODEBOOK.md` | Definitions and decision rules for every variable and code. |

## Reproducing the main figures

Each figure in Table I of the paper is a count of a code in `coding_v11.csv` divided by 139. For example:

```python
import pandas as pd
c = pd.read_csv("data/coding_v11.csv")
(c["label_leakage_risk"] == "yes").sum()          # 53 (38.1%)
(c["label_explanation"] == "explained").sum()      # 19 (13.7%)
flags = ["internal_inconsistency", "label_leakage_risk", "accuracy_ge_0_99_without_discussion",
         "bias_free_claim_without_measure", "superiority_claim_without_numbers"]
any_flag = (c["internal_inconsistency"] == "clear") | (c[flags[1:]] == "yes").any(axis=1)
any_flag.sum()                                     # 120 (86.3%)
```

## Notes

- **Evidence columns.** These contain verbatim quotations from the studies. Annotations that begin with `v9:`, `v11` or `v11 bukti` are coder notes added during re-verification, some written in Indonesian. Several quotations are joined with `||`.
- **Full texts are not included.** Copyright stays with the publishers; use the DOI links in `studies.csv`.
- **Nine studies coded from publisher web pages** were not re-checked in version 11 and keep their version 10 codes.

## License

The data and documentation are released under the Creative Commons Attribution 4.0 International license (CC BY 4.0); see `LICENSE`.
