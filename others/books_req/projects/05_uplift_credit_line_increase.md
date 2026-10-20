# Project 5: Uplift Model for a Credit-Line Increase (CLI) Campaign

**Skills shown:** Causal inference, uplift modelling, A/B experiment design, heterogeneous treatment effects, business ROI, decision optimisation
**Time:** 2–3 weeks · **Difficulty:** ⭐⭐⭐

---

## 1. Business Context

A credit-card issuer runs a quarterly **credit-line increase (CLI)** campaign that offers higher limits to existing customers. Today, CLI goes to everyone above a score threshold. The CFO has three concerns:

- **Wasted offers:** many customers would have spent more anyway (*sure things*), so the CLI adds risk without adding revenue.
- **Risk-only cases:** some customers don't respond at all, so the CLI only raises exposure (*lost causes*).
- **Harm:** a few customers become **more likely to default** after a CLI (*sleeping dogs*).

The question is **"Who should get the CLI so that incremental profit is maximised?"**, not "who is likely to spend?".

## 2. Problem Statement

Using data from a randomised experiment (treatment = offer, control = no offer), build an **uplift model** that estimates, for each customer, the **incremental effect** of the treatment on:

1. **Response / spend uplift:** additional spend or revolving balance (revenue).
2. **Risk uplift:** change in default probability (cost).

Combine both into an **incremental profit score** per customer. Then decide **who to target** under a budget or risk constraint, and show the ₹ gain compared with "target everyone" and "target by response model".

### Key questions to answer

- What is the average treatment effect (ATE) of the campaign? Is it statistically significant?
- How large is the heterogeneity? What share of customers are persuadables, sure things, lost causes, and sleeping dogs?
- How much incremental profit does uplift-based targeting add vs. the current policy?
- How would you design the next experiment to validate the model (sample size, power, guardrail metrics)?

## 3. Data

> No public dataset has bank CLI experiments, so use public randomised marketing datasets as **proxies**. Then add a **simulated credit-risk outcome** to model the trade-off between revenue uplift and risk uplift.

| Dataset | Link | Use |
|---|---|---|
| ⭐ Hillstrom Email (MineThatData) | https://blog.minethatdata.com/2008/03/minethatdata-e-mail-analytics-and-data.html | Classic randomised campaign with visit, conversion, and spend. Ideal starter |
| ⭐ Criteo Uplift Dataset | https://ailab.criteo.com/criteo-uplift-prediction-dataset/ | Large-scale (about 13M rows) RCT. Good for scale and Qini benchmarks |
| X5 RetailHero / Lenta / Megafon | Loaders in `scikit-uplift`: https://www.uplift-modeling.com/ | Extra uplift benchmarks |
| Bank Marketing (UCI) | https://archive.ics.uci.edu/dataset/222/bank+marketing | Banking context (not randomised). Use it to explain why observational data needs propensity methods |
| Simulated CLI data (self-built) | Python simulation with known true uplift | Validate methods against the ground truth, and add a default outcome |

**Recommended:** start with Hillstrom (spend as revenue), then build a **simulated CLI dataset** with known heterogeneous effects for both spend and default. That lets you prove your model recovers the true uplift.

## 4. Scope & Steps

| Phase | Tasks |
|---|---|
| 1. Experiment check | Check randomisation balance (SMD), sample sizes, ATE with confidence intervals (t-test / bootstrap) |
| 2. Baselines | Response model (traditional targeting) vs. random targeting |
| 3. Meta-learners | S-learner, T-learner, X-learner, DR-learner with LightGBM base models |
| 4. Direct uplift models | Uplift trees / causal forest (EconML `CausalForestDML`) |
| 5. Evaluation | Qini curve, AUUC, uplift by decile, calibration of predicted vs. observed uplift |
| 6. Risk uplift | Second uplift model for default probability (simulated outcome) |
| 7. Decision policy | Incremental profit = Δrevenue − Δexpected loss − offer cost. Rank and select under a budget or risk-appetite constraint |
| 8. Segmentation | Profile persuadables vs. sleeping dogs with SHAP on the uplift model |
| 9. Next experiment | Power analysis, stratified randomisation, guardrail metrics (default, complaints), duration |
| 10. Deliver | Executive memo (1 page), notebook/report, Streamlit "what-if" targeting simulator |

## 5. Success Metrics

| Type | Metric | Target |
|---|---|---|
| Model | Qini coefficient / AUUC | Beats the response-model targeting baseline |
| Validity | Uplift recovered on simulated data | Correlation with true uplift ≥ 0.7 |
| Business | Incremental profit vs. "target all" | ≥ 20% higher at the same or lower risk |
| Business | Sleeping-dog customers excluded | Identified and quantified |
| Communication | Exec memo clarity | 1 page, decision-first (Pyramid Principle) |

## 6. Tools & Tech Stack

| Layer | Tools |
|---|---|
| Data | pandas / Polars, DuckDB |
| Causal / uplift | `scikit-uplift` (sklift), `causalml` (Uber), `econml` (Microsoft), DoWhy |
| ML | LightGBM, scikit-learn |
| Stats | statsmodels, SciPy (t-tests, power analysis), bootstrap |
| Explainability | SHAP |
| Optimisation | PuLP / OR-Tools (budget-constrained targeting) |
| Tracking | MLflow |
| Demo | Streamlit |
| Quality | pytest, ruff, GitHub Actions |

## 7. Fintech Considerations

- **Responsible lending:** a CLI must not push customers into over-indebtedness. Include affordability checks (FOIR) as hard constraints.
- **Fairness:** check that targeting doesn't systematically exclude protected groups.
- **Regulation:** RBI rules on credit cards require explicit customer consent for limit increases. Mention it in the design.
- **Long-term effects:** short-term spend uplift may differ from 12-month profit, so note the need for a long-horizon holdout.

## 8. Stretch Goals

- Multi-treatment uplift (different CLI sizes: +20%, +50%, +100%)
- Off-policy evaluation of the new targeting policy (IPS / doubly robust)
- A sequential experiment / multi-armed bandit for offer allocation
- Use DoWhy to show refutation tests (placebo treatment, random common cause)

## 9. Resume Bullet Template

```
Built an uplift model (X-learner/causal forest) on randomised campaign data to target credit-line increases by incremental profit (Δspend − Δexpected loss); improved campaign profit by X% vs. blanket targeting, excluded Y% "sleeping dog" customers, and designed a powered follow-up experiment.
```

## 10. Related Books

Causal Inference in Python (Facure) · Trustworthy Online Controlled Experiments (Kohavi) · Practical Statistics for Data Scientists · Data Science for Business (Provost & Fawcett) · Storytelling with Data
