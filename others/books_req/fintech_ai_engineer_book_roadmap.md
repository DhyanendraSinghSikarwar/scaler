# Book Roadmap: Senior Data Scientist / AI Engineer, Fintech (70+ LPA)

**Target profile:** Senior/Staff/Lead DS or AI Engineer at a fintech, bank, NBFC, payments, or trading company.
**What these roles expect:** you build models, put them into production, run them on the cloud, and run GenAI/LLM systems. You also handle regulated data, explain models to risk and compliance teams, and drive business impact.

**Legend:**
- **T** = Theory (concepts) · **P** = Practical (code, hands-on)
- ⭐ = Read first if you're short on time
- [Free] = full book free online (official) · [Code] / [Resources] = book is paid, companion code or material is free
- ✅ **Read** = read these chapters fully · ❌ **Skip** = don't read these chapters

> **Chapter numbers** are for the edition named. Where I wasn't sure of the exact numbering, chapters are named by **topic** instead. If your edition differs, match by topic. (Your *Practical Statistics* PDF is the 2017 1st edition, which has the same 7 chapters.)
>
> Every required topic is covered in both theory and practice. See the **Topic Coverage Check** at the end of the book sections.

---

## 1. Statistics & Probability

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T+P ⭐ | Practical Statistics for Data Scientists | Bruce, Bruce & Gedeck | Inference, regression, resampling, with code | Ch 1 EDA, 2 Data & Sampling Distributions, 3 Experiments & Significance Testing, 4 Regression, 5 Classification | Ch 6 Statistical ML, 7 Unsupervised (covered by ISLP + Géron) |
| T ⭐ | An Introduction to Statistical Learning with Applications in Python (ISLP) · [Free](https://www.statlearning.com/) | James, Witten, Hastie, Tibshirani, Taylor | Industry-standard ML theory without heavy proofs | Ch 2 Statistical Learning, 3 Linear Regression, 4 Classification, 5 Resampling, 6 Regularization, 8 Trees, 11 Survival Analysis, 12 Unsupervised, 13 Multiple Testing (with the labs for each) | Ch 1 Intro, 7 Non-linearity, 9 SVM, 10 Deep Learning |
| T ⭐ | Trustworthy Online Controlled Experiments | Kohavi, Tang, Xu | A/B testing as it's done at scale | Ch 1 Intro, 2 Running & Analyzing Experiments, 3 Twyman's Law, 7 Metrics & OEC, 17 Statistics behind Experiments, 18 Variance Estimation, 19 A/A Test, 21 Sample Ratio Mismatch, 22 Leakage & Interference, 23 Long-term Effects | Ch 4–6, 8–16, 20 |
| T | Causal Inference: The Mixtape · [Free](https://mixtape.scunning.com/) | Scott Cunningham | Causal theory, intuitive | Ch 3 DAGs, 4 Potential Outcomes, 5 Matching, 9 Difference-in-Differences | Ch 1, 2, 6 RDD, 7 IV, 8 Panel Data, 10 Synthetic Control |
| P | Causal Inference in Python · [Free predecessor](https://matheusfacure.github.io/python-causality-handbook/) | Matheus Facure | Causal methods in Python code (uplift, DiD) | Causal basics, RCTs, Graphical Models, Regression, Propensity Scores, Effect Heterogeneity, Meta-learners, Difference-in-Differences | Synthetic Control and later advanced chapters |

## 2. Python, SQL & Software Engineering

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T ⭐ | Fluent Python (2nd ed.) | Luciano Ramalho | Deep Python: data model, dataclasses, generators, typing, async | Ch 1 Data Model, 2 Sequences, 3 Dicts & Sets, 5 Data Class Builders, 6 Object References & Mutability, 7 Functions as Objects, 8 Type Hints, 9 Decorators & Closures, 13 Protocols & ABCs, 17 Iterators & Generators, 18 with/match, 19 Concurrency Models, 20 Concurrent Executors, 21 Async | Ch 4, 10, 11, 12, 14, 15, 16, 22, 23, 24 |
| P ⭐ | Effective Python (latest ed.) | Brett Slatkin | "Do this, not that" rules. 1 rule a day | Pythonic Thinking, Lists & Dicts, Functions, Comprehensions & Generators, Classes & Interfaces, Concurrency & Parallelism, Robustness, Testing & Debugging | Metaclasses & Attributes, Collaboration |
| P ⭐ | Effective Pandas 2 | Matt Harrison | Fast, clean pandas | Series & DataFrame basics, dtypes (numeric, string, categorical, datetime), Method Chaining, Filtering, GroupBy/Aggregation, Pivot/Reshape, Merging, Time Series | Plotting and Excel/export chapters |
| T+P ⭐ | SQL for Data Analysis | Cathy Tanimura | Window functions, cohorts, experiments in SQL | Ch 2 Preparing Data, 3 Time Series, 4 Cohorts, 6 Anomaly Detection, 7 Experiment Analysis, 8 Complex Datasets | Ch 1, 5 Text Analysis, 9 Conclusion |
| T ⭐ | Architecture Patterns with Python · [Free](https://www.cosmicpython.com/book/preface.html) | Percival & Gregory | Clean, testable production code | Ch 1–7 (Domain Model, Repository, Abstractions, Service Layer, TDD, Unit of Work, Aggregates), 8 Events & Message Bus, 13 Dependency Injection | Ch 9–12 (Message Bus deep dive, Commands, Microservice Events, CQRS) |
| T+P | High Performance Python (latest ed.) | Gorelick & Ozsvald | Speed and memory | Profiling, Matrix & Vector Computation (NumPy/pandas), Compiling to C (Numba), multiprocessing, Using Less RAM (and the pandas/Polars chapter if your edition has one) | Lists/Tuples, Dicts/Sets, Iterators (covered by Fluent Python), Async I/O, Clusters, Lessons from the Field |
| P | Python Testing with pytest (2nd ed.) | Brian Okken | Testing ML code | Ch 1–7 (Getting Started → Strategy), 9 Coverage, 10 Mocking, 11 tox & CI | Ch 8, 12–16 |

## 3. Core Machine Learning

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| P ⭐ | Hands-On Machine Learning (3rd ed., Keras/TF; or the Scikit-Learn & PyTorch ed.) · [Code](https://github.com/ageron/handson-ml3) | Aurélien Géron | The best practical ML book | Ch 1 ML Landscape, 2 End-to-End Project, 3 Classification, 4 Training Models, 6 Decision Trees, 7 Ensembles & Boosting, 8 Dimensionality Reduction, 9 Unsupervised · Neural nets: 10 Intro to ANNs, 11 Training Deep Nets, 16 NLP with Attention/Transformers, 19 Training & Deploying at Scale | Ch 5 SVM, 12 Custom TF Models, 13 TF Data, 14 CNNs, 15 RNNs, 17 Autoencoders/GANs/Diffusion, 18 Reinforcement Learning |
| T | Interpretable Machine Learning · [Free](https://christophm.github.io/interpretable-ml-book/) | Christoph Molnar | SHAP, LIME, PDP for credit model approval | Interpretability intro, Linear Regression, Logistic Regression, Decision Tree, PDP, ALE, Permutation Feature Importance, LIME, Shapley Values, SHAP, Counterfactual Explanations | GLM/GAM, RuleFit, Feature Interaction, Global Surrogate, other example-based methods, Neural Network interpretation |
| P | Effective XGBoost | Matt Harrison | Gradient boosting for tabular fintech data | All chapters (short book) | — |

### How to read ISLP + Géron together

| Topic | ISLP (theory) | Géron (practice) |
|---|---|---|
| ML framing, bias–variance | Ch 2 | Ch 1–2 |
| Linear regression | Ch 3 | Ch 4 |
| Logistic regression, classification metrics | Ch 4 | Ch 3–4 |
| Cross-validation, bootstrap | Ch 5 | Ch 2 |
| Regularization (Ridge, Lasso) | Ch 6 | Ch 4 |
| Trees, random forest, boosting | Ch 8 | Ch 6–7 + Effective XGBoost |
| Unsupervised (PCA, clustering) | Ch 12 | Ch 8–9 |
| Survival analysis (time to default) | Ch 11 | ISLP Ch 11 lab |
| Multiple testing (A/B) | Ch 13 | ISLP Ch 13 lab |

**Order:** Practical Statistics Ch 1–5 → for each topic: ISLP chapter → Géron chapter → apply it to Project 1.

## 4. Deep Learning

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T ⭐ | Understanding Deep Learning · [Free](https://udlbook.github.io/udlbook/) | Simon J.D. Prince | Modern, clear DL theory | Ch 1–9 (Intro → Supervised Learning → Shallow & Deep Nets → Loss Functions → Fitting → Gradients & Init → Measuring Performance → Regularization), 11 Residual Networks, 12 Transformers, 13 Graph Neural Networks (fraud rings) | Ch 10 CNNs, 14–19 (Unsupervised, GANs, Flows, VAEs, Diffusion, RL), 20, 21 |

Practice for deep learning comes from Géron Ch 10, 11, 16, 19 (Section 3) and Raschka (Section 5).

## 5. LLMs, GenAI & AI Agents ⭐ (highest salary lever)

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T ⭐ | AI Engineering · [Resources](https://github.com/chiphuyen/aie-book) | Chip Huyen (2025) | The reference for apps on foundation models | All 10 chapters (Intro, Foundation Models, Evaluation Methodology, Evaluating AI Systems, Prompt Engineering, RAG & Agents, Finetuning, Dataset Engineering, Inference Optimization, Architecture & User Feedback) | — |
| T+P ⭐ | Build a Large Language Model (From Scratch) · [Code](https://github.com/rasbt/LLMs-from-scratch) | Sebastian Raschka | LLM internals, coded by hand | Ch 1–7 (Understanding LLMs, Text Data, Attention, GPT Model, Pretraining, Classification Fine-tuning, Instruction Fine-tuning), Appendix A (PyTorch), Appendix E (LoRA). Code along | Appendix B, C, D |
| P ⭐ | Hands-On Large Language Models · [Code](https://github.com/HandsOnLLM/Hands-On-Large-Language-Models) | Jay Alammar & Maarten Grootendorst | Embeddings, search, RAG, fine-tuning in code | Ch 1 Intro, 2 Tokens & Embeddings, 3 Inside LLMs, 4 Text Classification, 6 Prompt Engineering, 7 Advanced Text Generation, 8 Semantic Search & RAG, 10 Creating Embedding Models, 12 Fine-tuning Generation Models | Ch 5 Clustering/Topic Modeling, 9 Multimodal, 11 Fine-tuning Representation Models |
| P | LLM Engineer's Handbook | Paul Iusztin & Maxime Labonne | End-to-end LLMOps | Ch 1 Architecture, 4 RAG Feature Pipeline, 5 Supervised Fine-tuning, 7 Evaluating LLMs, 8 Inference Optimization, 9 RAG Inference Pipeline, 10 Deployment, 11 MLOps & LLMOps | Ch 2 Tooling Setup, 3 Data Engineering, 6 Preference Alignment |
| P | Prompt Engineering for LLMs | John Berryman & Albert Ziegler | Systematic prompt design | Ch 4–10 (Designing LLM Apps, Prompt Content, Assembling the Prompt, Taming the Model, Conversational Agency, LLM Workflows, Evaluating LLM Apps) | Ch 1–3 (basics, covered by AI Engineering), 11 Looking Ahead |
| P | Building Applications with AI Agents | Michael Albada | Agent design and production | All chapters (fundamentals, tool use, orchestration & planning, memory, multi-agent, evaluation, security, production) | — |
| T+P | Developer's Playbook for Large Language Model Security | Steve Wilson | OWASP LLM Top 10 for BFSI | Prompt Injection, Sensitive Information Disclosure, Supply Chain & RAG risks, Excessive Agency, Overreliance/Hallucination, the closing framework chapter | Background/history chapters |

## 6. MLOps & ML System Design

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T ⭐ | Designing Machine Learning Systems · [Resources](https://github.com/chiphuyen/dmls-book) | Chip Huyen | The best book on production ML | All 11 chapters | — |
| T ⭐ | Machine Learning Design Patterns | Lakshmanan, Robinson, Munn | Reusable solutions (feature store, rebalancing) | Ch 2 Data Representation, 3 Problem Representation, 5 Resilient Serving, 6 Reproducibility, 7 Responsible AI | Ch 1 Intro, 4 Model Training, 8 Connected Patterns |
| P | Practical MLOps | Noah Gift & Alfredo Deza | CI/CD, containers, monitoring in practice | Ch 1 Intro, 2 Foundations, 3 Containers, 4 Continuous Delivery, 6 Monitoring & Logging, 7 MLOps for AWS | Ch 5 AutoML, 8 Azure, 9 GCP, 10 Interoperability, 11 CLI/Microservices, 12 Case Studies |
| P ⭐ | Machine Learning System Design Interview | Ali Aminian & Alex Xu | Senior ML design rounds | Ch 1 Framework, 5 Harmful Content Detection (≈ fraud), 6 Video Recommendation, 7 Event Recommendation, 8 Ad Click Prediction (≈ real-time scoring), 10 Personalized News Feed, 11 People You May Know (graph) | Ch 2 Visual Search, 3 Street View Blurring, 4 Video Search, 9 Similar Listings |

## 7. Data Engineering & Distributed Systems

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T ⭐ | Designing Data-Intensive Applications | Martin Kleppmann | Consistency, streaming, storage (payments and ledgers) | Ch 1 Reliable/Scalable Systems, 2 Data Models, 3 Storage & Retrieval, 5 Replication, 6 Partitioning, 7 Transactions, 9 Consistency & Consensus, 10 Batch Processing, 11 Stream Processing | Ch 4 Encoding, 8 Distributed Systems Trouble, 12 Future. (1st-ed. numbers; match by topic in the 2nd ed.) |
| T | Fundamentals of Data Engineering | Joe Reis & Matt Housley | Modern data stack lifecycle | Ch 1 DE Described, 2 Lifecycle, 3 Good Architecture, 5 Source Systems, 6 Storage, 7 Ingestion, 8 Queries/Modeling/Transformation, 9 Serving, 10 Security & Privacy | Ch 4 Choosing Technologies, 11 Future |
| P | Learning Spark (2nd ed.) | Damji et al. | Large-scale data processing | Ch 1–5 (Intro → Spark SQL & DataFrames), 7 Optimizing & Tuning, 8 Structured Streaming, 9 Delta Lake, 10 MLlib | Ch 6 Datasets (Scala), 11 Deploying ML Pipelines, 12 Spark 3.0 |
| P | Kafka: The Definitive Guide (2nd ed.) | Shapira et al. | Real-time event streaming | Ch 1 Meet Kafka, 3 Producers, 4 Consumers, 6 Internals, 7 Reliable Delivery, 8 Exactly-Once, 14 Stream Processing | Ch 2 Installing, 5 Admin API, 9 Pipelines, 10 Mirroring, 11 Security, 12 Administering, 13 Monitoring |
| T | System Design Interview, Vol. 1 | Alex Xu | Backend design fluency | Ch 1 Scale to Millions, 2 Estimation, 3 Framework, 4 Rate Limiter, 5 Consistent Hashing, 6 Key-Value Store, 7 Unique ID Generator, 8 URL Shortener, 10 Notification System | Ch 9, 11–15 |
| T ⭐ | System Design Interview, Vol. 2 | Alex Xu & Sahn Lam | Fintech-specific designs | Distributed Message Queue, Metrics Monitoring, Ad Click Event Aggregation, **Payment System, Digital Wallet, Stock Exchange** | All other chapters |

## 8. Cloud & Infrastructure

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| P ⭐ | Data Science on AWS | Chris Fregly & Antje Barth | SageMaker end-to-end (AWS is the most common cloud in Indian fintech) | Ch 1 Intro, 4 Ingest, 6 Prepare Dataset, 7 Train, 8 Train at Scale, 9 Deploy, 10 Pipelines & MLOps, 11 Streaming, 12 Security | Ch 2 Use Cases, 3 AutoML, 5 Explore. (The book is from 2021, so check current screens in AWS docs) |
| P ⭐ | Generative AI on AWS | Fregly, Barth, Eigenbrode | Bedrock, fine-tuning, RAG on AWS | Ch 1 Use Cases, 2 Prompt Engineering, 5 Fine-tuning & Evaluation, 6 PEFT/LoRA, 8 Deployment Optimizations, 9 RAG & Agents, 12 Amazon Bedrock | Ch 3, 4, 7 RLHF, 10–11 Multimodal/Stable Diffusion |
| T+P | Docker Deep Dive | Nigel Poulton | Containers | The Big Picture, Images, Containers, Containerizing an App, Docker Compose, Networking, Volumes, Security | Installing Docker, Docker Engine internals, Swarm, Stacks, Overlay Networking |
| T+P | Kubernetes: Up and Running (3rd ed.) | Burns, Beda, Hightower, Evenson | Model serving at scale | Ch 1 Intro, 2 Containers, 4 kubectl, 5 Pods, 6 Labels, 7 Service Discovery, 8 Ingress, 9 ReplicaSets, 10 Deployments, 12 Jobs, 13 ConfigMaps & Secrets | Ch 3, 11, 14–22 |
| T+P | Terraform: Up & Running (3rd ed.) | Yevgeniy Brikman | Infrastructure as code | Ch 1 Why Terraform, 2 Getting Started, 3 State, 4 Modules, 5 Loops & Conditionals, 6 Secrets | Ch 7–10 |

> **Pair with a certification:** AWS Certified Machine Learning Engineer – Associate, or AWS AI Practitioner. A certification signals cloud depth to recruiters.

## 9. Fintech Domain Knowledge (your differentiator)

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T ⭐ | Credit Risk Analytics | Baesens, Rösch, Scheule | PD/LGD/EAD, scorecards, Basel | Intro to Credit Risk Analytics, Exploratory Data Analysis, Data Preprocessing, Credit Scoring (PD), LGD, EAD, Model Validation, Stress Testing | Intro to SAS (redo the SAS code in Python), Low-Default Portfolios, Default Correlation / portfolio models |
| P | Intelligent Credit Scoring (2nd ed.) | Naeem Siddiqi | How scorecards are built in industry | Development process, Data Review & Project Parameters, Development Database, Scorecard Development (binning, WoE/IV, logistic, reject inference, scaling), Choosing Cut-offs, Management Reports, Validation | Organisational roles, Infrastructure design chapters |
| T+P ⭐ | Fraud Analytics Using Descriptive, Predictive, and Social Network Techniques | Baesens, Van Vlasselaer, Verbeke | Fraud and AML modeling | Ch 1 Fraud Detection & Analytics, 2 Data Collection & Preprocessing, 3 Descriptive Analytics, 4 Predictive Analytics, 5 Social Network Analysis, the Post-Processing chapter (alerts, backtesting, monitoring) | Remaining broader-perspective chapters |
| T | Rebooting India | Nandan Nilekani & Viral Shah | India Stack context (Aadhaar, UPI) | Chapters on Aadhaar/identity, digital payments, and direct benefit transfer | All other chapters |
| T | Fairness and Machine Learning · [Free](https://fairmlbook.org/) | Barocas, Hardt, Narayanan | Bias in lending decisions | Introduction, Classification (fairness criteria), Testing Discrimination in Practice | Legitimacy, Relative Notions, Causality, US Law, Broader View, Datasets |

> **Also read (not books, but mandatory):** RBI Digital Lending Guidelines, the RBI FREE-AI framework (2025), the DPDP Act 2023, the US Fed SR 11-7 (model risk management), and the EU AI Act sections on credit scoring.

## 10. Soft Skills: Communication, Leadership, Influence

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| T+P ⭐ | Storytelling with Data | Cole Nussbaumer Knaflic | Turning models into decisions | All 10 chapters (short) | — |
| T ⭐ | Data Science for Business | Provost & Fawcett | ROI and expected-value framing | Ch 1 Data-Analytic Thinking, 2 Business Problems, 7 Expected Value, 8 Visualizing Model Performance, 11 Toward Analytical Engineering, 13 DS & Business Strategy | Ch 3–6, 9, 10, 12, 14 (technical, covered elsewhere) |
| T | The Pyramid Principle | Barbara Minto | Top-down structured communication | Part One: Logic in Writing | Parts Two–Four |
| P | Writing for Busy Readers | Todd Rogers & Jessica Lasky-Fink | Concise emails, memos, docs | All chapters (short) | — |
| T ⭐ | The Staff Engineer's Path | Tanya Reilly | Senior/staff scope, influence, vision | All 9 chapters | — |
| P | Crucial Conversations | Patterson, Grenny, et al. | High-stakes disagreements | Start with Heart, Learn to Look, Make It Safe, Master My Stories, STATE My Path, Explore Others' Paths, Move to Action | Remaining chapters |
| P ⭐ | Never Split the Difference | Chris Voss | Negotiating the 70 LPA offer | All 10 chapters | — |

## 11. Interview Prep (to convert skills into the offer)

| Type | Book | Author(s) | Why | ✅ Read | ❌ Skip |
|---|---|---|---|---|---|
| P ⭐ | Ace the Data Science Interview | Nick Singh & Kevin Huo | Stats, SQL, ML, product-sense questions | All chapters (solve every problem) | — |
| P | Generative AI System Design Interview | Ali Aminian & Hao Sheng | LLM, RAG, GenAI design rounds | Framework/Intro, Smart Compose, Chatbot/Personal Assistant, Retrieval-Augmented Generation | Image captioning, Image/Face generation, Text-to-Image, Headshots, Text-to-Video |
| T | Grokking Algorithms (2nd ed.) | Aditya Bhargava | Visual DSA intuition before NeetCode | Binary Search & Big O, Arrays & Linked Lists, Recursion, Quicksort, Hash Tables, BFS/Graphs, Trees, Greedy, Dynamic Programming | Dijkstra, Balanced Trees, k-NN, Where to Go Next |

ML System Design Interview is listed in Section 6.

---

## Books Removed (nothing extra to read: already covered by the books above)

| Book | Covered by |
|---|---|
| Mathematics for Machine Learning | ISLP + Practical Statistics + UDL Ch 1–9 |
| Python for Data Analysis | Effective Pandas 2 |
| Robust Python | Fluent Python Ch 5, 8, 13 |
| The Hundred-Page ML Book | ISLP + Géron |
| Deep Learning with Python (Chollet), Deep Learning with PyTorch (Godoy) | UDL + Géron Part II + Raschka |
| Reliable ML, Machine Learning Engineering (Burkov) | Designing ML Systems + Practical MLOps |
| Data Science on GCP | Data Science on AWS (AWS chosen) |
| Python for Finance, ML in Finance, ML for Algorithmic Trading, Advances in Financial ML | Only needed for quant/trading roles |
| The FINTECH Book, Inspired | Data Science for Business + Rebooting India |

---

## Topic Coverage Check (every topic: theory + practice)

| # | Topic | Theory (read) | Practice (do) |
|---|---|---|---|
| 1 | EDA & descriptive stats | Practical Stats Ch 1 | Practical Stats Ch 1 code · Project 1 EDA |
| 2 | Probability & sampling | Practical Stats Ch 2 | Ace the DS Interview: Probability problems |
| 3 | Hypothesis testing & A/B tests | Practical Stats Ch 3 · Kohavi Ch 1–3, 17–19, 21 · ISLP Ch 13 | SQL for Data Analysis Ch 7 · ISLP Ch 13 lab · Project 5 |
| 4 | Linear & logistic regression | ISLP Ch 3–4 · Practical Stats Ch 4–5 | Géron Ch 3–4 · code both from scratch in NumPy |
| 5 | Model evaluation, cross-validation | ISLP Ch 5 | Géron Ch 2–3 |
| 6 | Regularization | ISLP Ch 6 | Géron Ch 4 |
| 7 | Trees, random forest, boosting | ISLP Ch 8 | Géron Ch 6–7 · Effective XGBoost · Project 1 |
| 8 | Unsupervised learning | ISLP Ch 12 | Géron Ch 8–9 |
| 9 | Survival analysis (time to default) | ISLP Ch 11 | ISLP Ch 11 lab |
| 10 | Interpretability (SHAP, LIME) | Molnar | Project 1 (SHAP reason codes) |
| 11 | Causal inference & uplift | Mixtape Ch 3, 4, 5, 9 | Facure · Project 5 |
| 12 | Deep learning fundamentals | UDL Ch 1–9, 11 | Géron Ch 10–11 |
| 13 | Transformers & LLM internals | UDL Ch 12 · AI Engineering Ch 2 | Raschka Ch 1–5 (code along) · Géron Ch 16 |
| 14 | Prompt engineering | AI Engineering Ch 5 | Prompt Engineering for LLMs Ch 4–10 · Hands-On LLMs Ch 6–7 |
| 15 | Embeddings, RAG | AI Engineering Ch 6 | Hands-On LLMs Ch 2, 8, 10 · LLM Engineer's Handbook Ch 4, 9 · Project 3 |
| 16 | Fine-tuning (LoRA, instruction tuning) | AI Engineering Ch 7–8 | Raschka Ch 6–7 + App. E · Hands-On LLMs Ch 12 · LLM Eng. Handbook Ch 5 |
| 17 | LLM evaluation | AI Engineering Ch 3–4 | LLM Eng. Handbook Ch 7 · Project 3 golden set |
| 18 | AI agents | AI Engineering Ch 6 | Albada (all) · Project 4 |
| 19 | LLM security & guardrails | LLM Security Playbook | Project 3 & 4 guardrails |
| 20 | Inference optimisation, cost | AI Engineering Ch 9 | LLM Eng. Handbook Ch 8, 10 · GenAI on AWS Ch 8 |
| 21 | ML system design | Designing ML Systems · ML Design Patterns | ML System Design Interview · Project 2 |
| 22 | MLOps (CI/CD, monitoring, drift) | Designing ML Systems Ch 7–10 | Practical MLOps Ch 1–4, 6 · LLM Eng. Handbook Ch 11 · Project 2 |
| 23 | Data engineering & distributed systems | DDIA · Fundamentals of DE | Learning Spark · Project 2 |
| 24 | Streaming | DDIA Ch 11 | Kafka TDG Ch 3–8, 14 · Project 2 |
| 25 | Backend & payment system design | System Design Interview Vol. 1–2 | Mock design rounds · Project 2 architecture |
| 26 | Python mastery | Fluent Python | Effective Python · refactor your own code weekly |
| 27 | Clean architecture & testing | Architecture Patterns with Python | pytest book · tests in all 5 projects |
| 28 | Performance | High Performance Python | Profile and optimise Project 2 |
| 29 | pandas | Effective Pandas 2 | StrataScratch · LeetCode Pandas |
| 30 | SQL | SQL for Data Analysis | DataLemur · LeetCode SQL 50 |
| 31 | DSA | Grokking Algorithms | NeetCode 150 |
| 32 | AWS ML & GenAI | DS on AWS Ch 1 · GenAI on AWS Ch 1, 12 | DS on AWS Ch 4, 6–12 · GenAI on AWS Ch 5–6, 9 · Project 4 · AWS certification |
| 33 | Docker, Kubernetes, Terraform | Docker / K8s / Terraform concept chapters | Same books' hands-on chapters · Projects 2 & 4 |
| 34 | Credit risk (PD/LGD/EAD, scorecards) | Credit Risk Analytics | Intelligent Credit Scoring · Project 1 |
| 35 | Fraud & AML | Fraud Analytics Ch 1, 3–5 · UDL Ch 13 (GNN) | Fraud Analytics Ch 2 + post-processing · Project 2 |
| 36 | Fairness & regulation | Fairness & ML · RBI/DPDP documents | Fairlearn in Project 1 · Project 3 (RBI corpus) |
| 37 | Indian fintech context | Rebooting India | Project 3 & 4 |
| 38 | Business framing & ROI | Data Science for Business | ₹ impact section in every project README |
| 39 | Data storytelling | Storytelling with Data | Charts in project READMEs · monthly presentation |
| 40 | Writing | Pyramid Principle Part One | Writing for Busy Readers · weekly 1-page memo |
| 41 | Leadership | The Staff Engineer's Path | Own one project end to end · write design docs |
| 42 | Difficult conversations | Crucial Conversations | Apply in one real disagreement a month |
| 43 | Negotiation | Never Split the Difference | Salary role-plays · Coursera negotiation course |
| 44 | Interview readiness | Ace the DS Interview · GenAI SD Interview | Weekly timed mock interview |

---

## Free Courses (can replace paid books)

| Topic | Course |
|---|---|
| LLMs | [Hugging Face LLM Course](https://huggingface.co/learn/llm-course) |
| MLOps | [Made With ML](https://madewithml.com/) |
| Deep learning | [Dive into Deep Learning](https://d2l.ai/) |
| Forecasting | [Forecasting: Principles and Practice](https://otexts.com/fpp3/) |
| Negotiation | Successful Negotiation (University of Michigan, Coursera, free to audit) |

> **Paid books cheaply:** check ISB library access to O'Reilly Learning, or buy Indian editions (Shroff/SPD, Wiley India), usually ₹800–1,500.

---

## Suggested Reading Order (about 12 months, alongside a job)

| Phase | Months | Focus | Books |
|---|---|---|---|
| 1. Foundations | 1–2 | Stats, SQL, Python | Practical Statistics · SQL for Data Analysis · Fluent Python |
| 2. Core ML | 3–4 | ML, interpretability, fintech basics | Géron · ISLP (selected chapters) · Molnar · Credit Risk Analytics |
| 3. Production | 5–6 | MLOps, system design | Designing ML Systems · ML Design Patterns · Architecture Patterns with Python |
| 4. LLM / GenAI | 7–9 | LLM internals and applications | Raschka · Hands-On LLMs · AI Engineering · LLM Engineer's Handbook |
| 5. Cloud & Scale | 10–11 | AWS, data infrastructure | Data Science on AWS · Generative AI on AWS · DDIA (selected chapters) |
| 6. Leverage | 12 | Communication, interviews, negotiation | Storytelling with Data · Staff Engineer's Path · Ace the DS Interview · Never Split the Difference |

## If You Read Only 10 Books

1. Practical Statistics for Data Scientists
2. Hands-On Machine Learning (Géron)
3. Designing Machine Learning Systems (Huyen)
4. AI Engineering (Huyen)
5. Build a Large Language Model (From Scratch) (Raschka)
6. Hands-On Large Language Models (Alammar & Grootendorst)
7. Designing Data-Intensive Applications (Kleppmann)
8. Credit Risk Analytics (Baesens)
9. Fraud Analytics (Baesens)
10. Storytelling with Data (Knaflic)

## Portfolio Projects to Pair with Reading (proof for the 70+ LPA bar)

| Project | Skills Shown |
|---|---|
| Credit scorecard + XGBoost challenger with SHAP-based reason codes | Stats, ML, interpretability, regulation |
| Real-time fraud detection (Kafka → feature store → model API → drift monitoring) | MLOps, streaming, system design |
| RAG assistant over RBI circulars with an evaluation harness and guardrails | LLM, RAG, evals, LLM security |
| Agentic KYC/document-processing workflow on AWS Bedrock | Agents, cloud, production GenAI |
| Uplift model for a credit-line increase or offer campaign | Causal inference, business ROI |

---

## Interview Practice Platforms

Books build understanding. These platforms build interview speed. Aim for ~40% books and ~60% timed practice.

| Area | Platform | What to do |
|---|---|---|
| DSA ⭐ | [NeetCode 150](https://neetcode.io/practice) | Easy–medium problems, by pattern. Blind 75 if short on time |
| DSA (intuition) | Grokking Algorithms (book, Aditya Bhargava) | Visual intro before NeetCode |
| SQL ⭐ | [DataLemur](https://datalemur.com/) | Real company SQL questions, from easy to hard |
| SQL | [LeetCode SQL 50](https://leetcode.com/studyplan/top-sql-50/) | Window functions, joins, aggregations |
| Pandas ⭐ | [StrataScratch](https://www.stratascratch.com/) | Same questions solvable in SQL and pandas |
| Pandas | [LeetCode Introduction to Pandas](https://leetcode.com/studyplan/introduction-to-pandas/) | Core pandas operations |
| Python concepts | Fluent Python + Effective Python | dataclass vs. NamedTuple, generators, decorators |
| ML coding | Géron + NumPy | Implement linear/logistic regression and k-means from scratch |
| Stats, ML, product sense ⭐ | Ace the Data Science Interview (book) | Question bank across all rounds |
| ML/GenAI system design ⭐ | ML System Design Interview + GenAI System Design Interview (books) | Fraud, ranking, and RAG designs |
| Mock interviews | [Pramp](https://www.pramp.com/) / peers / Claude as interviewer | 1 timed mock per week |
| Company-specific | Glassdoor, LeetCode Discuss, AmbitionBox | Recent interview experiences at your target companies |
