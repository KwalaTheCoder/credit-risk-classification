# Audit and refactor

Removes full-data correlation screening, global outlier deletion and SMOTE-before-CV. Uses fold-local imputation/encoding, safe income ratios and class weighting. F1 is no longer incorrectly negated. Probability-based ROC AUC and average precision accompany macro F1. Audit loan_grade and other attributes for availability at the actual decision time.

Original algorithms and historical outputs remain in legacy/. No production dataset was available during refactoring; new real-data metrics remain unmeasured. See README.md for the reproducible evaluation protocol.
