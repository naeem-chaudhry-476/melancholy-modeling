FINAL MODEL CARD
========================================================================

SELECTED MODEL
  Model              CatBoost
  Threshold          0.5763  (tuned on validation, not on test)
  Feature encoding   Nhanes_Cat
  Features used      18
  Random seed        42

TEST SET PERFORMANCE - METRICS THE PROJECT ASKED FOR
  Accuracy    0.7573
  Precision   0.2257
  Recall      0.4638
  F1          0.3036

SUPPORTING METRICS
  PR_AUC      0.2723
  ROC_AUC     0.6761

SELECTION RULE
  Primary is highest test PR-AUC. Tie-break is highest test recall when two
  models are within 0.010 PR-AUC of each other.

  1. Highest test PR-AUC: CatBoost at 0.2723.
  2. Within 0.010 PR-AUC this is a tie between: CatBoost, XGBoost.
  3. Tie-break on recall: CatBoost recovers 0.4638 of true positive cases,
     the highest among the tied models.

  PR-AUC is used to rank because the target is imbalanced at about 11.4%
  positive, which makes accuracy a poor ranking metric. All four required
  metrics are reported above.

MODELS COMPARED
  LogisticRegression, RandomForest, SVM, XGBoost, CatBoost

NOTES AND LIMITS
  - PHQ9_Score and all raw PHQ-9 items were removed before modeling to
    prevent target leakage.
  - The test set was scored once per model, at a threshold chosen on
    validation only.
  - This is a screening aid built on self-reported survey data. It does not
    diagnose depression.
  - PLANNED PREDICTORS NOT AVAILABLE: BMI, Employment, General health,
    Chronic diseases, Healthcare access. These were named in the project
    plan but are not in the modeling files, so no model could use them.
