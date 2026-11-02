# Real-data evaluation — 2026-11-02

Input: `Credit_Risk/credit_risk_dataset.csv`. Dataset SHA-256: `ce3c6d2167717bf1627d1c0c81cbccd28323cd4aa7b96d542599366d5ff6aac8`.

Model selection used only training-fold CV. One final holdout was reserved before preprocessing. The full CV leaderboard, dependency versions, seed, feature schema and split sizes are in [experiments/report.json](experiments/report.json).

- Model: **RandomForestClassifier**
- Training rows: **26067**; holdout rows: **6514**
- Selection metric: `f1_macro`; selected CV score: **0.886225**
- Dummy baseline CV score on the same folds: **0.439222**
- Five CV folds; seed 42; tuning: **False**
- Identical raw predictor rows grouped: **True**

| Metric | Holdout value |
|---|---:|
| accuracy | 0.927541 |
| balanced_accuracy | 0.854485 |
| f1_macro | 0.885872 |
| roc_auc | 0.927815 |
| average_precision | 0.881672 |

Accuracy 95% Wilson interval: [0.9210, 0.9336]. This treats holdout observations as independent; grouped duplicates reduce the effective sample size, so the interval is descriptive rather than a complete uncertainty estimate.


## Dataset-specific interpretation

Missing employment length and loan interest rates are imputed inside folds. Identical raw predictor rows stay in the same split/CV fold, including conflicting labels. Loan grade and other credit variables require a decision-time availability audit; these results do not establish suitability for lending decisions.

## Reproduce

Activate an environment with the project's requirements, change to the repository root, and run:

```cmd
python -m mlproject train --data "D:\data\Credit_Risk\credit_risk_dataset.csv" --output artifacts --folds 5 --seed 42
```

Results were obtained with the dependency versions recorded in the JSON report. Different versions can change results slightly. Negative MAE selection scores are higher-is-better. CV scores are selection estimates and optimistic; the holdout is the final internal result. Hyperparameter tuning is enabled on the smaller datasets and diabetes; for credit, soccer, student and churn the default bounded candidate comparison is used. Scores are not directly comparable across datasets or tasks.

Do not tune against the now-observed holdout or claim improvement over the legacy notebooks: leakage, split policies and preprocessing differ. Raw data, row-level predictions and trained pickle artifacts are not included in this evidence commit.
