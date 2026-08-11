# AML-Fraud-Detection-Risk-Scoring-Model
A machine learning pipeline that uses transaction data to predict fraudulent activity and auto-generate SHAP audit explanations for flagged alerts.

#Built using pandas, scikit-learn, XGBoost, and SHAP.

#What This Project Does
In real-world Anti-Money Laundering (AML) workflows, models can't just be accurate—they have to be explainable for compliance officers and regulatory audits.

#This repository implements:
Feature engineering based on banking accounting errors (e.g., balance mismatches post-transfer).
Unsupervised anomaly detection (Isolation Forest) to tag suspicious patterns without requiring labels.
Supervised risk scoring (XGBoost with class weighting) to handle extreme imbalanced data ($0.13\%$ fraud).
Automated compliance reports using SHAP waterfall plots to explain why a specific transaction was flagged.

#Dataset
This project uses the PaySim Financial Fraud Dataset (~6.36 million rows).
#Target Variable: isFraud ($0 = \text{Normal}$, $1 = \text{Fraud}$)
#Class Imbalance: 99.87% normal / 0.13% fraud
#Focus Areas: High-risk payment types (TRANSFER and CASH_OUT)

#Pipeline Breakdown

1.Domain Feature EngineeringRather than relying only on raw transaction amounts, the pipeline derives balance discrepancy features:
errorBalanceOrig: Difference between expected sender balance and actual post-transaction balance.

errorBalanceDest: Difference between expected recipient balance and actual post-transaction balance.

amount_to_oldbalance_ratio: Ratio of transfer amount relative to initial sender balance.

2. Hybrid Machine Learning Strategy

Isolation Forest: Fits on transaction distributions to compute an anomaly_score. This score acts as an extra feature for the supervised classifier.

XGBoost Classifier: Uses scale_pos_weight to heavily penalize missing fraud cases due to the severe class imbalance. Evaluated using ROC-AUC and Precision/Recall instead of standard accuracy.

3. Model Governance & Explainability (SHAP)

#Beeswarm Plots: Show global feature impact across all test cases.
#Waterfall Plots: Breakdown log-odds impact for individual flagged transactions to generate audit reports.

#Key ResultsROC-AUC: ~0.999
#Top Feature: errorBalanceOrig (Accounting discrepancies during full account drainage are the strongest indicator of fraud).
