# Project 2: Real-Time Transaction Fraud Detection System

**Skills shown:** MLOps, streaming, feature stores, low-latency model serving, monitoring and drift detection, ML system design
**Time:** 5–6 weeks · **Difficulty:** ⭐⭐⭐⭐

---

## 1. Business Context

A payments company processes about 2 million card and UPI transactions a day. Fraud losses are about 8 bps of GMV, and the current rules engine has two weaknesses:

- **It misses new fraud patterns,** for example account takeover and mule accounts.
- **It produces too many false positives,** which block genuine customers, drive complaints, and lose revenue.

The fraud team needs an **ML-based, real-time scoring system**. Every transaction must be scored in **< 100 ms (p99)** before authorisation, and the system has to adapt as fraud patterns drift.

## 2. Problem Statement

Design and build an **end-to-end real-time fraud detection platform** that:

1. **Ingests** a stream of transactions through Kafka or Redpanda.
2. **Computes real-time features** such as transaction velocity, amount vs. user average, new device or merchant flags, and time since last transaction. It serves them from an online feature store and keeps them consistent with offline training features.
3. **Scores** each transaction with a gradient-boosting model behind a low-latency API and returns **approve / step-up (OTP) / decline**.
4. **Monitors** data drift, prediction drift, latency, and fraud-capture rate, and **alerts** on degradation.
5. **Retrains** automatically through a pipeline, using a champion/challenger promotion step.

The system has to balance **fraud loss avoided against customer friction**, so pick thresholds by cost, not by accuracy.

### Key questions to answer

- What is fraud capture (recall) at a 0.5% decline rate?
- What is the ₹ saved after subtracting the cost of false positives?
- How quickly does the model degrade when fraud patterns shift, and how is that detected?
- Can p99 latency stay under 100 ms at 500 transactions per second?

## 3. Data

| Dataset | Link | Use |
|---|---|---|
| ⭐ IEEE-CIS Fraud Detection | https://www.kaggle.com/c/ieee-fraud-detection | Primary. Rich transaction and identity features |
| ⭐ Sparkov Simulated Transactions | https://www.kaggle.com/datasets/kartik2112/fraud-detection | Has user IDs and timestamps, which makes it ideal for velocity features and stream replay |
| PaySim (mobile money) | https://www.kaggle.com/datasets/ealaxi/paysim1 | Mobile-money transfer fraud, close to UPI-style flows |
| Credit Card Fraud (ULB) | https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud | Classic benchmark (PCA features). For quick baselines only |
| Bank Account Fraud (NeurIPS 2022) | https://www.kaggle.com/datasets/sgpjesus/bank-account-fraud-dataset-neurips-2022 | Account-opening fraud, with fairness attributes |

**Recommended:** use Sparkov or PaySim for the streaming simulation, because they have user IDs and time, and IEEE-CIS for model depth.

## 4. Architecture

```
Transaction Simulator (replays dataset)
        │
        ▼
   Kafka / Redpanda  ──►  Stream processor (Bytewax / Faust / Flink)
        │                         │ computes real-time features
        │                         ▼
        │                  Online Feature Store (Redis via Feast)
        ▼                         │
  Scoring API (FastAPI) ◄─────────┘
        │  model loaded from MLflow Registry
        ▼
  Decision: approve / OTP / decline  ──► Kafka "decisions" topic
        │
        ▼
  Offline store (Parquet / DuckDB)  ──►  Training pipeline (Prefect/Airflow)
        │                                      │
        ▼                                      ▼
  Monitoring (Evidently + Prometheus + Grafana)   Champion/Challenger → MLflow Registry
```

## 5. Scope & Steps

| Phase | Tasks |
|---|---|
| 1. Offline modelling | EDA, time-based split (no random split), feature engineering, LightGBM baseline, cost-based threshold selection |
| 2. Imbalance handling | Class weights vs. under-sampling, evaluated by PR-AUC rather than ROC-AUC |
| 3. Feature store | Define features in Feast. Offline store = Parquet, online store = Redis. Check training/serving consistency |
| 4. Streaming | Kafka producer replays transactions in time order. Stream job updates velocity features |
| 5. Serving | FastAPI scoring service, model from the MLflow registry, load test with Locust |
| 6. Monitoring | Evidently drift reports, Prometheus metrics, Grafana dashboard (latency, score distribution, alert rate) |
| 7. Retraining | Prefect flow: pull labels → retrain → evaluate vs. champion → promote if better |
| 8. Deploy | Docker Compose locally, then Kubernetes (kind/minikube) or AWS ECS. CI/CD with GitHub Actions |
| 9. Business report | Cost matrix (fraud loss vs. friction cost), ₹ saved, recommended thresholds |

## 6. Success Metrics

| Type | Metric | Target |
|---|---|---|
| Model | PR-AUC | Beats the rules baseline by ≥ 30% |
| Model | Recall at 0.5% decline rate | ≥ 60% |
| System | p99 latency | < 100 ms |
| System | Throughput | ≥ 500 TPS on a laptop or small instance |
| Ops | Drift alert fires within | 1 day of an injected drift |
| Business | Net ₹ saved per 1M transactions | Reported |

## 7. Tools & Tech Stack

| Layer | Tools |
|---|---|
| Streaming | Kafka or Redpanda, Bytewax / Faust (Python) or Apache Flink |
| Feature store | Feast + Redis |
| ML | LightGBM / XGBoost, scikit-learn, Optuna |
| Tracking & registry | MLflow |
| Orchestration | Prefect or Airflow |
| Serving | FastAPI, Uvicorn, Docker |
| Load testing | Locust |
| Monitoring | Evidently, Prometheus, Grafana |
| Infra | Docker Compose → Kubernetes / AWS ECS, Terraform (stretch) |
| CI/CD | GitHub Actions |

## 8. Fintech Considerations

- **Label delay:** chargebacks arrive 30–90 days late. Explain how training windows account for this.
- **Explainability:** each decline should log its top SHAP features for the analyst review queue.
- **Feedback loop:** declined transactions get no label, so describe the bias this creates and how to handle it (e.g., a small random "explore" approval bucket).
- **PCI-DSS:** never log raw card numbers. Hash or tokenise them.

## 9. Stretch Goals

- Graph features (shared device or IP across accounts) with NetworkX or a GNN for mule detection
- An autoencoder or Isolation Forest for unknown fraud patterns
- Shadow deployment and A/B rollout of the challenger
- An LLM "analyst copilot" that summarises why a transaction was flagged

## 10. Resume Bullet Template

```
Designed and built a real-time fraud detection platform (Kafka → Feast/Redis → FastAPI → LightGBM) scoring 500+ TPS at <100 ms p99, with Evidently drift monitoring and automated champion/challenger retraining; improved fraud capture by X% at a fixed 0.5% decline rate, saving an estimated ₹Y Cr/yr.
```

## 11. Related Books

Designing ML Systems (Huyen) · ML Design Patterns · Reliable ML · Fraud Analytics (Baesens) · Kafka: The Definitive Guide · Designing Data-Intensive Applications
