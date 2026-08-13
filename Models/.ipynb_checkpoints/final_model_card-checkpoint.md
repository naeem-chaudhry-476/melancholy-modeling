# Final Model Card

**Selected model:** CatBoost
**Decision threshold:** 0.5486 (tuned on validation, not on test)
**Feature encoding:** Nhanes_Cat
**Features used:** 23
**Random seed:** 42

## Test set performance

| Metric | Value |
| --- | --- |
| PR_AUC | 0.2734 |
| ROC_AUC | 0.6817 |
| Recall | 0.5277 |
| Precision | 0.2191 |
| F1 | 0.3096 |
| Accuracy | 0.7316 |

## Selection rule

Primary: highest test PR-AUC. Tie-break: highest test recall when two models are
within 0.010 PR-AUC of each other.

1. Highest test PR-AUC: CatBoost at 0.2734.
2. Within 0.010 PR-AUC this is a tie between: CatBoost, XGBoost.
3. Tie-break on recall: CatBoost recovers 0.5277 of true positive cases, the highest among the tied models.

## Models compared

LogisticRegression, RandomForest, SVM, XGBoost, CatBoost

## Notes and limits

- The target is imbalanced at about 11.4% positive, so PR-AUC leads and accuracy is not used to rank models.
- `PHQ9_Score` and all raw DPQ items were removed before modeling to prevent target leakage.
- The test set was scored once per model, at a threshold chosen on validation only.
- This is a screening aid built on survey data. It does not diagnose depression.
- **Fairness caveat:** this model was trained on `INDFMMPI, WTMEC_COMB`, which the other models dropped. Its win is provisional until it is retrained on the same feature set.
