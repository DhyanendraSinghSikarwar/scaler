# Fintech Portfolio Projects: Index

These five projects together cover the skills hiring managers check for at the 70+ LPA level. Build them in this order, because each one reuses parts of the one before.

| # | Project | Core skills | Time | Difficulty |
|---|---|---|---|---|
| 1 | [Credit Scorecard + XGBoost Challenger](01_credit_scorecard_xgboost.md) | Stats, ML, interpretability, regulation | 3–4 weeks | ⭐⭐ |
| 2 | [Real-Time Fraud Detection System](02_realtime_fraud_detection.md) | MLOps, streaming, system design | 5–6 weeks | ⭐⭐⭐⭐ |
| 3 | [RBI Regulatory RAG Assistant](03_rbi_regulatory_rag.md) | LLM, RAG, evals, LLM security | 3–4 weeks | ⭐⭐⭐ |
| 4 | [Agentic KYC Document Processing](04_agentic_kyc_processing.md) | Agents, cloud, production GenAI | 4–5 weeks | ⭐⭐⭐⭐ |
| 5 | [Uplift Model for Credit-Line Increase](05_uplift_credit_line_increase.md) | Causal inference, experimentation, ROI | 2–3 weeks | ⭐⭐⭐ |

## Common Standards for Every Project

| Area | Standard |
|---|---|
| Repo | One GitHub repo per project, with a README covering the problem, an architecture diagram, results, and how to run it |
| Code | `src/` layout, type hints, `ruff`, `mypy`, `pytest` (aim for >70% coverage on core logic) |
| Environment | `uv` or `poetry`, a `Dockerfile`, `Makefile` targets |
| Experiments | MLflow tracking |
| CI | A GitHub Actions workflow that runs lint, tests, and build on every push |
| Docs | A 1-page "model card" or "system card" per project |
| Impact | Each README states the business result in ₹ or % terms, even if estimated |

> **Note:** All dataset links were given from memory. Check each one before you start. If one has moved, search the dataset name on Kaggle, UCI, or Hugging Face.
