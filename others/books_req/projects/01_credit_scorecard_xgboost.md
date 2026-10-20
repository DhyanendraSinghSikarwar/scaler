# Project 1: Credit Scorecard + XGBoost Challenger with SHAP Reason Codes

**Skills shown:** Statistics, classical ML, gradient boosting, interpretability, model risk management, credit-risk domain knowledge
**Time:** 3–4 weeks · **Difficulty:** ⭐⭐

---

## 1. Business Context

A digital NBFC lends unsecured personal loans of ₹50K–₹5L to salaried and thin-file customers. Its current approval policy is a rule-based filter (bureau score > 700, income > ₹25K). Two problems follow:

- **Default rate is high:** 90+ DPD (days past due) is about 6% on the current book, which is above its risk appetite.
- **Good customers are rejected:** thin-file and new-to-credit applicants are turned away even when other signals show low risk.

The Chief Risk Officer wants a statistically sound, explainable credit-risk model. Under RBI guidelines and internal model-risk policy, every rejection must come with **clear reason codes** for the customer and the auditors.

## 2. Problem Statement

Build a **Probability of Default (PD) model** that predicts whether an applicant will reach 90+ DPD within 12 months of disbursal. Deliver it in two forms:

1. **Champion: a logistic-regression scorecard.** It uses Weight of Evidence (WoE) binning and is converted to a points-based score (e.g., 300–900). This is the regulator-friendly, fully transparent model.
2. **Challenger: an XGBoost/LightGBM model.** It is tuned for higher discrimination and paired with **SHAP-based reason codes** to make it explainable.

Compare the two on predictive power, stability, fairness, and business impact. Then recommend which one to deploy, or a hybrid such as the scorecard for approve/decline and XGBoost for pricing.

### Key questions to answer

- How much does the challenger improve AUC/Gini and KS over the scorecard?
- At a fixed approval rate, how much lower is the bad rate?
- At a fixed bad rate, how many more customers can be approved, and what is that worth in ₹?
- Are the top reason codes intuitive and consistent with credit policy?
- Is the model stable over time (PSI) and fair across groups such as gender and age band?

## 3. Data

| Dataset | Link | Use |
|---|---|---|
| ⭐ Home Credit Default Risk | https://www.kaggle.com/c/home-credit-default-risk | Primary. Multi-table (bureau, previous applications, installments), close to real NBFC data |
| Lending Club Loan Data | https://www.kaggle.com/datasets/wordsforthewise/lending-club | Alternative. Large, with loan-level outcomes |
| Give Me Some Credit | https://www.kaggle.com/c/GiveMeSomeCredit | Smaller. Good for quick scorecard practice |
| German Credit (UCI) | https://archive.ics.uci.edu/dataset/144/statlog+german+credit+data | Tiny. Good for learning WoE/IV by hand |
| Default of Credit Card Clients (UCI) | https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients | Credit-card variant with repayment history |

**Recommended:** use Home Credit. Join the auxiliary tables to engineer bureau and behavioural features.

## 4. Scope & Steps

| Phase | Tasks |
|---|---|
| 1. Problem framing | Define the target (90+ DPD in 12 months), observation and performance windows, and exclusions. Write a 1-page model development plan |
| 2. EDA & data quality | Missingness, outliers, leakage check (drop any post-disbursal features), class imbalance |
| 3. Feature engineering | Aggregations from bureau and installment tables (counts, ratios, recency, utilisation). Debt-to-income, delinquency flags |
| 4. Scorecard (champion) | Coarse/fine classing → WoE → Information Value (IV) filtering → logistic regression → scaling to points (PDO = 20, base score 600) |
| 5. Challenger | LightGBM/XGBoost with Optuna tuning, monotonic constraints on key features, probability calibration (isotonic/Platt) |
| 6. Explainability | SHAP global importance, SHAP-based top-3 adverse reason codes per rejected applicant |
| 7. Validation | Out-of-time (OOT) test, AUC/Gini, KS, calibration plot, PSI/CSI, decile lift table |
| 8. Fairness | Approval rate and bad rate by protected-group proxies, disparate impact ratio |
| 9. Business impact | Cut-off strategy: approval rate vs. bad rate trade-off curve, expected loss = PD × LGD × EAD, ₹ profit simulation |
| 10. Deliver | FastAPI scoring endpoint, model card, Streamlit "credit officer" demo |

## 5. Success Metrics

| Type | Metric | Target |
|---|---|---|
| Model | Gini (OOT) | Scorecard ≥ 0.50; challenger ≥ +5 points over scorecard |
| Model | KS | ≥ 35 |
| Stability | PSI (train vs. OOT) | < 0.10 |
| Calibration | Brier score / calibration slope | Close to 1.0 |
| Business | Bad-rate reduction at the same approval rate | ≥ 15% relative |
| Explainability | Reason codes generated for 100% of declines | ✅ |

## 6. Tools & Tech Stack

| Layer | Tools |
|---|---|
| Data | pandas / Polars, DuckDB |
| Scorecard | `optbinning` (WoE/IV binning), `scorecardpy`, statsmodels |
| ML | scikit-learn, LightGBM / XGBoost, Optuna |
| Explainability | SHAP |
| Fairness | Fairlearn |
| Tracking | MLflow |
| Serving | FastAPI, Pydantic, Docker |
| Demo | Streamlit |
| Quality | pytest, ruff, mypy, GitHub Actions |

## 7. Regulatory & Model-Risk Considerations

- **Explainability:** an adverse action must be explainable to the customer (RBI fair practices code).
- **Model documentation:** follow an SR 11-7-style structure: purpose, data, methodology, limitations, validation, monitoring.
- **Fairness:** don't use protected attributes as features. Test them only for bias.
- **Monitoring plan:** monthly PSI, quarterly back-testing, and recalibration triggers.

## 8. Stretch Goals

- Reject inference (parceling or fuzzy augmentation)
- LGD and EAD models to complete expected loss
- Risk-based pricing (interest rate by PD band)
- An LLM-generated plain-language decline letter built from SHAP reason codes

## 9. Resume Bullet Template

```
Built a PD credit scorecard (logistic/WoE) and an XGBoost challenger with SHAP reason codes on 300K+ loan applications; challenger improved Gini by X pts and cut bad rate by Y% at constant approval rate, worth an estimated ₹Z Cr/yr in avoided losses.
```

## 10. Related Books

Credit Risk Analytics (Baesens) · Intelligent Credit Scoring (Siddiqi) · Interpretable ML (Molnar) · Practical Statistics for Data Scientists · Effective XGBoost
