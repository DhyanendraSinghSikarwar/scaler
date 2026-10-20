# Project 3: RBI Regulatory RAG Assistant with Evaluation and Guardrails

**Skills shown:** LLM application engineering, retrieval-augmented generation (RAG), evaluation, guardrails, LLM security, cost and latency optimisation
**Time:** 3–4 weeks · **Difficulty:** ⭐⭐⭐

---

## 1. Business Context

Compliance teams at banks and NBFCs track hundreds of RBI circulars, master directions, and notifications on KYC, digital lending, outsourcing, IT governance, and more. Analysts lose hours searching PDFs to answer questions like:

- *"What is the cooling-off period in the digital lending guidelines?"*
- *"What are the re-KYC timelines for high-risk customers?"*

A wrong answer can mean a regulatory penalty.

The Chief Compliance Officer wants an internal assistant that answers regulatory questions **accurately and with citations**, and **refuses when unsure**.

## 2. Problem Statement

Build a **production-grade RAG assistant** over RBI (and optionally SEBI) regulatory documents that:

1. **Ingests** circulars and master directions (PDF/HTML), tracking metadata: circular number, date, department, and whether it has been superseded.
2. **Retrieves** relevant passages with hybrid search (BM25 + dense embeddings) and a re-ranker.
3. **Answers** with an LLM that **cites the exact circular and paragraph**, and says *"I don't know"* when the retrieved evidence is insufficient.
4. **Prefers the latest applicable regulation** when circulars conflict or one supersedes another.
5. Is **evaluated systematically** on a golden question set for retrieval quality, faithfulness, and answer correctness.
6. Is **guarded** against prompt injection, out-of-scope questions, and PII leakage.

### Key questions to answer

- Which chunking strategy and retriever setup gives the best recall@k?
- How often does the model hallucinate, and how much do the guardrails and re-ranking reduce it?
- What are the cost and latency per query, and how much does caching save?

## 3. Data

| Source | Link | Use |
|---|---|---|
| ⭐ RBI Notifications / Circulars | https://www.rbi.org.in/Scripts/NotificationUser.aspx | Primary corpus |
| ⭐ RBI Master Directions | https://www.rbi.org.in/Scripts/BS_ViewMasterDirections.aspx | Consolidated, authoritative rules (KYC, digital lending, etc.) |
| RBI FAQs | https://www.rbi.org.in/Scripts/FAQDisplay.aspx | Ready-made Q&A pairs to seed the golden evaluation set |
| SEBI Circulars (optional) | https://www.sebi.gov.in/ | Extend to capital markets |
| FinanceBench (eval reference) | https://github.com/patronus-ai/financebench | Example of a financial QA evaluation design |

**Start small:** 30–50 documents on 3 topics (KYC, Digital Lending, Fair Practices Code), then scale up.

**Golden set:** write 100–150 Q&A pairs by hand or semi-automatically, each with its expected source paragraph. Include unanswerable and adversarial questions.

## 4. Architecture

```
PDF/HTML circulars
   │  parse (Docling / PyMuPDF / Unstructured)
   ▼
Chunking (section-aware, 500–800 tokens, with metadata)
   │
   ▼
Embeddings ──► Vector DB (Qdrant / pgvector)  +  BM25 index
                       │
User query ──► Input guardrail ──► Query rewrite ──► Hybrid retrieve (top 20)
                                                      │
                                                      ▼
                                                 Re-ranker (top 5)
                                                      │
                                                      ▼
                                   LLM (Claude / GPT / open model) with citation prompt
                                                      │
                                                      ▼
                                  Output guardrail (grounding check, PII) ──► Answer + citations
                                                      │
                                                      ▼
                                  Tracing & eval (Langfuse / Phoenix, RAGAS)
```

## 5. Scope & Steps

| Phase | Tasks |
|---|---|
| 1. Ingestion | Scrape or download docs, parse tables and sections, store metadata (number, date, superseded flag) |
| 2. Baseline RAG | Fixed-size chunks + dense retrieval + LLM. Measure on the golden set |
| 3. Improve retrieval | Section-aware chunking, hybrid BM25 + dense, re-ranker, metadata filters (date, department) |
| 4. Improve generation | Citation-forcing prompt, "answer only from context" instruction, structured output (answer, citations, confidence) |
| 5. Evaluation | Retrieval: recall@k, MRR. Generation: faithfulness, answer relevance, correctness (RAGAS + LLM-as-judge + human spot-check) |
| 6. Guardrails | Prompt-injection detection, topic restriction, PII redaction, refusal on low retrieval scores |
| 7. Ops | Semantic cache, token/cost tracking, latency budget, tracing |
| 8. Deliver | FastAPI backend + Streamlit/Chainlit UI, Docker, eval report in the README |

## 6. Success Metrics

| Type | Metric | Target |
|---|---|---|
| Retrieval | Recall@5 | ≥ 90% |
| Generation | Faithfulness (grounded in context) | ≥ 95% |
| Generation | Answer correctness (golden set) | ≥ 85% |
| Safety | Correct refusal on unanswerable questions | ≥ 90% |
| Safety | Prompt-injection attacks blocked | ≥ 95% of the test suite |
| Ops | p95 latency | < 5 s |
| Ops | Cost per query | Reported (₹) |

## 7. Tools & Tech Stack

| Layer | Tools |
|---|---|
| Parsing | Docling, PyMuPDF, Unstructured |
| Embeddings | bge / e5 / gte (open), or OpenAI / Voyage / Cohere embeddings |
| Vector DB | Qdrant or Postgres + pgvector |
| Keyword search | BM25 (`rank_bm25`) or OpenSearch |
| Re-ranker | bge-reranker or Cohere Rerank |
| LLM | Claude (Anthropic API / AWS Bedrock), GPT, or an open model via Ollama/vLLM |
| Framework | LlamaIndex or LangChain/LangGraph, or plain Python (recommended, to show you understand the internals) |
| Evaluation | RAGAS, DeepEval, a custom LLM-as-judge |
| Guardrails | Guardrails AI / NeMo Guardrails / Llama Guard, Presidio (PII) |
| Observability | Langfuse or Arize Phoenix |
| Serving & UI | FastAPI, Streamlit / Chainlit, Docker |

## 8. Fintech & Security Considerations

- **Versioning:** regulations get superseded. Store effective dates and always show the circular date in answers.
- **Not legal advice:** the UI should state that answers are for internal reference and need compliance sign-off.
- **Data residency:** for real bank use, discuss on-prem or India-region hosting (DPDP Act).
- **OWASP LLM Top 10:** document the mitigations for prompt injection, sensitive information disclosure, and excessive agency.

## 9. Stretch Goals

- A change-detection agent that flags new circulars weekly and summarises the impact
- Multi-hop questions ("compare re-KYC rules for low- vs. high-risk customers")
- Fine-tuning the embedding model on regulatory text and measuring the uplift
- Hindi query support

## 10. Resume Bullet Template

```
Built a production RAG assistant over 500+ RBI circulars (hybrid BM25+dense retrieval, re-ranking, citation-grounded answers, guardrails); achieved 92% recall@5 and 96% faithfulness on a 150-question golden set, cutting compliance research time by ~70%.
```

## 11. Related Books

AI Engineering (Huyen) · Hands-On LLMs (Alammar) · LLM Engineer's Handbook · Prompt Engineering for LLMs · Developer's Playbook for LLM Security
