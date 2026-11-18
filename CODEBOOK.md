# Codebook

**General rule.** Code what the study writes, not what may be inferred. A statement about other studies, or about machine learning in general (typically in the introduction or literature review), does not count as the study's own label, data, result, or claim. Every code other than the default ("no", "not explained", "not mentioned") is supported by a verbatim quotation in `coding_v11.csv`.

## DA1 — Validity of the performance label

The label is the target variable the model predicts. If a study has several targets, the one presented as performance is coded.

| Variable | Codes and rules |
|---|---|
| `label_explanation` (F1) | **explained**: the study states both who or what produced the label (supervisor, appraisal department, operational record, questionnaire, platform rating) and its form (scale, classes, period, or formula). **partly explained**: only one of the two, typically a benchmark field name with its values (e.g. “PerformanceRating 3 or 4”) without the rater. **not explained**: neither. A field name alone without values, a list of input features, or a definition of performance quoted from the literature does not count. |
| `circular_or_proxy_label` (F2) | **yes**: the label is computed from, or clustered (e.g. k-means) on, indicators that are also model inputs; or the modelled outcome is not performance (attrition, promotion, AI adoption, a training score) although the study calls it performance prediction. |
| `own_label_reliability_discussed` (F3) | **yes**: the study discusses the reliability or bias of the ratings it uses as its own label (recency, leniency, inter-rater agreement, measurement error). General criticism of human appraisal does not count (that is F4). Missing-data handling does not count. |
| `motivation_criticizes_rater_subjectivity` (F4) | **yes**: the introduction or background uses the subjectivity, bias, or inconsistency of traditional appraisal as a reason for using ML. |

## DA2 — Provenance of the data

| Variable | Codes and rules |
|---|---|
| `data_source` (F5) | One code for the main training data, checked in this order. **not stated or unclear**: no source, two incompatible sources, or a placeholder. **public benchmark dataset**: Kaggle, GitHub, UCI, RPubs or similar, even if presented as company data. **synthetic/simulated**: generated or simulated data. **survey/questionnaire**: questionnaire responses rather than records. **real organization, named or placed**: the organization is named, or identified by sector and country or region (“a mid-sized commercial bank in Indonesia”). **real organization, unnamed**: a real organization is claimed with less information (“a multinational company”). Columns resembling IBM HR Analytics without a stated source are coded not stated. |

## DA3 — Rigor of model evaluation

| Variable | Codes and rules |
|---|---|
| `validation_design` (F6) | **random hold-out split**: data divided into training and test sets. **k-fold cross-validation**: k-fold used to estimate performance without a separate test set. **hold-out split + cross-validation**: both. **temporal validation**: trained on an earlier period, tested on a later one. **test set mentioned, formation not described**: results on a test or validation set, or “data can be divided”, without saying how. **not reported**: no evaluation on unseen data. Cross-validation used only to tune hyperparameters does not count. |
| `internal_inconsistency` (F7) | Abstract, method, tables, figures and conclusion are compared: best model and figures, F1 = 2PR/(P+R), RMSE = √MSE, confusion-matrix totals, sample size, data source, models evaluated. **clear**: two parts contradict each other, a value is impossible (e.g. F1 outside the range of precision and recall), or a placeholder or template text remains. **minor**: a small difference or a mislabel that does not change the conclusion. **none**: nothing found, or a single obvious typo. Differences between different models are not inconsistencies. Only *clear* counts as a rigor flag; *minor* is a sensitivity check. |
| `label_leakage_risk` (F8) | **yes**: an input feature is a rating from an earlier period, a sub-score of the label, or a field known to encode it (the salary-hike percentage in IBM HR Analytics when not stated as removed, or promotions awarded on the basis of the rating); or the label is computed from the inputs. **no** if the study states such a field was removed. |
| `accuracy_ge_0_99_without_discussion` (F9) | **yes**: the headline test metric of the proposed model (accuracy, F1, AUC, or R²) is ≥ 0.99 and the study does not discuss leakage or overfitting for that result. Training accuracy, single-class precision, secondary metrics, and figures from other studies do not count; 98.5% is below the threshold. |
| `bias_free_claim_without_measure` (F10) | **yes**: the study states, as a result, discussion of its findings, or conclusion, that its model or its decisions are fair, unbiased, impartial, or equitable, and reports no fairness measure (demographic parity, equal opportunity, disparate impact, or per-subgroup metrics). Aims (“aims to reduce bias”), recommendations, and claims about ML in general do not count. |
| `superiority_claim_without_numbers` (F11) | **yes**: the study states that its model outperforms other, existing, or traditional methods without comparison values in text, tables or figures, including comparisons given only qualitatively (e.g. “Low/Moderate → High”), and models claimed superior that are never evaluated. Promotional wording without a comparison does not count. |

A study is counted as having **at least one rigor flag** when it has a *clear* internal inconsistency or any of F8–F11.

## DA4 — Explainability

| Variable | Codes and rules |
|---|---|
| `explainability` (F12) | Decided by what is **displayed** (a figure, table, values, or a feature ranking reported in the results), not by what is named. **formal method output**: SHAP, LIME, permutation importance, or partial dependence output. **proxy**: attention weights, built-in feature importance, decision-tree rules, or regression coefficients presented as an explanation. **claimed without a method**: the model is called explainable or interpretable, or a method is named, but no output is shown. **not mentioned**: none of these, including explainability mentioned only as future work. Formal and proxy together are coded formal. |
| `explanations_evaluated` (F13) | **yes**: the quality of the explanations themselves is evaluated (fidelity, stability, or a user or expert study). |

## Reliability check

The first author re-coded 20 studies blind to the AI-assisted first pass: 16 drawn at random from the 130 PDFs and 4 added for rarely used codes. Every disagreement was adjudicated against the full text. The first-pass error patterns found there (F5 coded as not stated, F8, F10, F11, F12) were then re-checked in the other full-text studies. See `blind_recoding_sample.csv` and `changes_v10_to_v11.csv`.
