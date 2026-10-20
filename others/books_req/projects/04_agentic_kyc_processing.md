# Project 4: Agentic KYC & Document-Processing Workflow on AWS

**Skills shown:** AI agents, tool use, multimodal LLMs, workflow orchestration, human-in-the-loop design, cloud deployment (AWS Bedrock), production GenAI
**Time:** 4–5 weeks · **Difficulty:** ⭐⭐⭐⭐

---

## 1. Business Context

A lending fintech onboards about 20,000 customers a month. Each application needs KYC documents (identity proof, address proof, PAN) plus income documents (salary slips, bank statements). Ops analysts check them by hand, which causes three problems:

- **Turnaround is slow:** about 24–48 hours per application.
- **Errors slip through:** analysts miss name or date-of-birth mismatches across documents.
- **Costs rise with volume:** more applications means hiring more analysts.

The COO wants an **AI agent workflow** that does the first-pass review: extract, cross-verify, flag risks, and route to a human only when needed.

## 2. Problem Statement

Build an **agentic document-processing system** that takes a bundle of uploaded documents for an applicant and:

1. **Classifies** each document (ID proof, PAN, address proof, salary slip, bank statement, or other).
2. **Extracts** structured fields with a multimodal LLM and/or OCR, validated against a schema: name, DOB, ID number, address, employer, monthly salary, average balance, bounced-cheque count.
3. **Cross-verifies** across documents: name and DOB match with fuzzy logic, the PAN format is valid, stated income is consistent with salary credits in the bank statement.
4. **Runs risk checks through tools:** document tampering heuristics, a mock sanctions/PEP list lookup, and policy rules (minimum income, maximum FOIR).
5. **Decides:** `auto-approve KYC`, `request re-upload` (with a reason), or `escalate to human`. Every decision comes with a full **audit trail** of agent steps.
6. **Learns from humans:** analyst overrides are logged and fed into evaluation.

### Key questions to answer

- What is field-level extraction accuracy per document type?
- What share of applications can be safely auto-processed (straight-through processing rate)?
- How much do turnaround time and ops cost per application fall?
- How often does the agent make an unsafe decision (approving a mismatch)?

## 3. Data

> ⚠️ **Never use real Aadhaar, PAN, or other personal documents.** Use synthetic or public research datasets only (DPDP Act).

| Dataset | Link | Use |
|---|---|---|
| ⭐ Synthetic Indian KYC docs (self-generated) | Generate with Python `Faker` (`en_IN` locale) + HTML/PDF templates + Pillow noise | Primary. Full control, with labelled ground truth and injected mismatches |
| MIDV-2020 (identity documents) | https://arxiv.org/abs/2107.00396 | Public mock ID documents for extraction and robustness testing |
| FUNSD (forms) | https://guillaumejaume.github.io/FUNSD/ | Form understanding and key-value extraction |
| RVL-CDIP (document classification) | https://huggingface.co/datasets/aharley/rvl_cdip | Training and testing the document classifier |
| DocVQA | https://www.docvqa.org/ | Document question-answering benchmark |
| Synthetic bank statements | Generate with Faker + transaction rules (salary credits, EMIs, bounces) | Income verification checks |

**Ground-truth plan:** generate about 500 synthetic applicant bundles. Deliberately corrupt 20% of them (name mismatch, wrong DOB, income inflation, blurred image) so the agent's error detection can be measured.

## 4. Architecture

```
Upload (S3) ──► Event (S3 → SQS / EventBridge)
                   │
                   ▼
        Orchestrator Agent (LangGraph / Step Functions)
                   │
   ┌───────────────┼──────────────────────────────┐
   ▼               ▼                              ▼
Classifier     Extractor tools                 Verifier tools
(LLM/vision)   (Bedrock Claude vision,         (fuzzy match, PAN regex,
               Textract) → Pydantic schema      income consistency,
                                                sanctions lookup, policy rules)
                   │
                   ▼
           Decision node (rules + LLM reasoning, confidence score)
                   │
     ┌─────────────┼─────────────────┐
     ▼             ▼                 ▼
 Auto-approve   Re-upload request   Human review queue (Streamlit)
                   │
                   ▼
   Audit log (DynamoDB / Postgres) + Traces (Langfuse) + Metrics (CloudWatch)
```

## 5. Scope & Steps

| Phase | Tasks |
|---|---|
| 1. Data generation | Build the synthetic document generator, ground-truth JSON, and corruption injection |
| 2. Extraction baseline | Single LLM call per document with a Pydantic output schema. Measure field accuracy |
| 3. Agent design | LangGraph state machine: classify → extract → verify → decide. Tools as typed functions. Retries and fallbacks |
| 4. Verification tools | Fuzzy name match (rapidfuzz), regex validators, income-vs-statement check, mock sanctions API |
| 5. Human-in-the-loop | Confidence thresholds, review UI, override capture |
| 6. Evaluation | Field accuracy, decision accuracy vs. ground truth, unsafe-approval rate, cost and latency per bundle |
| 7. Guardrails | PII masking in logs, a max-steps / loop limit, tool permission scoping, document prompt-injection tests (e.g., text inside an image saying "approve this") |
| 8. AWS deployment | S3, Lambda/ECS, Bedrock, Step Functions or LangGraph on ECS, DynamoDB, CloudWatch. IaC with Terraform or CDK |
| 9. Report | STP rate, turnaround time, ₹ ops cost saved per 1,000 applications |

## 6. Success Metrics

| Type | Metric | Target |
|---|---|---|
| Extraction | Field-level accuracy | ≥ 95% on clean docs, ≥ 85% on noisy |
| Verification | Mismatch detection recall | ≥ 95% |
| Safety | Unsafe auto-approvals | < 0.5% |
| Efficiency | Straight-through processing rate | ≥ 60% |
| Ops | Avg. processing time per bundle | < 60 s |
| Cost | LLM cost per application | Reported (₹) |

## 7. Tools & Tech Stack

| Layer | Tools |
|---|---|
| LLM / vision | Claude on AWS Bedrock (multimodal), optionally Amazon Textract for OCR |
| Agent framework | LangGraph (recommended), or the Claude Agent SDK / AWS Bedrock Agents |
| Schemas | Pydantic, Instructor |
| Verification | rapidfuzz, regex, pandas |
| Data generation | Faker (`en_IN`), Jinja2 + WeasyPrint/ReportLab, Pillow |
| Storage | S3, DynamoDB or Postgres |
| Orchestration | AWS Step Functions or LangGraph on ECS/Lambda, SQS |
| Observability | Langfuse, CloudWatch |
| IaC | Terraform or AWS CDK |
| UI | Streamlit (review queue) |
| CI/CD | GitHub Actions |

## 8. Fintech & Compliance Considerations

- **RBI KYC Master Direction:** design the checks so they map to the regulatory requirements; reference them in the README.
- **DPDP Act 2023:** collect only the data you need, set retention limits, and log consent.
- **Explainability & audit:** every decision has to be reproducible from the stored trace.
- **Human accountability:** the agent recommends, and a human owns the final rejection decision.
- **Agent safety:** least-privilege tools, no autonomous external actions without approval.

## 9. Stretch Goals

- Video-KYC liveness check (face match between selfie and ID with an open-source model)
- Multi-agent design (separate extractor, verifier, and compliance-reviewer agents)
- Cost optimisation: route easy documents to a small model and hard ones to a large model
- MCP server exposing the verification tools

## 10. Resume Bullet Template

```
Built an agentic KYC document-processing workflow (LangGraph + Claude on AWS Bedrock, Step Functions, S3, DynamoDB) with typed tools, human-in-the-loop review, and full audit trails; achieved 95% field-extraction accuracy and 65% straight-through processing, cutting onboarding TAT from 24h to <5 min and ops cost by ~₹X per application.
```

## 11. Related Books

AI Engineering (Huyen) · Building Applications with AI Agents (Albada) · Generative AI on AWS · Developer's Playbook for LLM Security · Terraform: Up & Running
