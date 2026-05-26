# AI Vendor GDPR Checklist

AI vendors (model providers, AI-as-a-service, AI-powered SaaS features) are processors or joint controllers under GDPR when personal data flows through them. They carry materially different risks from standard SaaS processors and require specific checks beyond a standard DPA review.

**Trigger this checklist when the codebase contains**:
- Calls to OpenAI, Anthropic, Google Gemini, Cohere, Mistral, AWS Bedrock, Azure OpenAI, Hugging Face Inference API, or similar
- Any `llm`, `ai`, `ml_model`, `embedding`, `generate`, `chat_completion`, `infer` patterns in service calls
- AI-powered features: summarisation, classification, extraction, recommendations, chatbot
- On-premise or self-hosted model serving (Ollama, vLLM, TGI)

---

## Table of Contents
1. [Risk categories specific to AI vendors](#1-risk-categories-specific-to-ai-vendors)
2. [Checklist: per-vendor checks](#2-checklist-per-vendor-checks)
3. [Standard AI vendor DPA reference](#3-standard-ai-vendor-dpa-reference)
4. [Training data risk](#4-training-data-risk)
5. [Art. 22 automated decision-making](#5-art-22-automated-decision-making)
6. [EU AI Act overlay](#6-eu-ai-act-overlay)
7. [Gap analysis additions](#7-gap-analysis-additions)
8. [Common AI vendor status table](#8-common-ai-vendor-status-table)

---

## 1. Risk Categories Specific to AI Vendors

| Risk | Why AI vendors are different |
|------|------------------------------|
| **Training data ingestion** | Some AI APIs use customer input to improve/retrain their models by default — this is processing personal data for a purpose beyond the original collection |
| **Output data residency** | Prompts and responses may be stored in the vendor's infrastructure in a jurisdiction outside EEA |
| **Inference of special category data** | AI models may infer health, political views, sexuality, religion from text or behaviour even when no explicit special category data is sent |
| **Automated decisions with legal/significant effects** | AI-driven scoring, classification, or recommendations may trigger Art. 22 |
| **Sub-processor depth** | AI vendors often use cloud sub-processors (e.g. OpenAI uses Azure) — the chain may be long and opaque |
| **Model provenance** | Open-source models fine-tuned on personal data may create data protection obligations even if self-hosted |

---

## 2. Checklist: Per-Vendor Checks

For each AI vendor found in the codebase, run these checks:

### 2.1 Data Processing Agreement

| Check | Pass condition | Gap signal |
|-------|---------------|-----------|
| DPA available from vendor | Yes | No DPA = cannot use vendor for EU personal data |
| DPA covers GDPR Art. 28 requirements | Yes | Check for: instructions, confidentiality, security, sub-processors, deletion, audit rights |
| DPA covers UK GDPR if UK users | Yes | Separate UK addendum or UK-equivalent terms |
| DPA signed / accepted? | Yes (record acceptance date and version) | Not accepted = DPA is not in effect |

### 2.2 Training Data Opt-Out

| Check | How to verify | Gap signal |
|-------|--------------|-----------|
| Does vendor use inputs to train models? | Check vendor data usage policy / API terms | Default opt-in to training = processing for second purpose |
| Is opt-out enabled? | Check API config / account settings / DPA terms | Opt-out available but not enabled = data being used for training |
| Is opt-out permanent or per-request? | API docs | Per-request opt-out is error-prone; account-level opt-out preferred |
| Does DPA explicitly exclude training use? | DPA Section on permitted purposes | Missing clause = ambiguous |

**Code scan signal**: Look for API calls without explicit `training=false`, `allow_training: false`, or equivalent parameter. Check vendor docs for the correct opt-out mechanism.

### 2.3 Data Residency

| Check | Pass condition | Gap signal |
|-------|---------------|-----------|
| Where are prompts/responses stored? | EEA, UK, or adequacy country | US/non-adequate country without transfer mechanism = Art. 44 violation |
| Does vendor offer EU data residency? | Yes (where required) | No EU option = must use SCCs or DPF |
| Transfer mechanism documented? | SCCs / DPF / Adequacy | None = unlawful transfer |
| Is vendor DPF-certified (if US)? | Check dataprivacyframework.gov | Not certified = SCCs required |

### 2.4 Sub-Processor Transparency

| Check | Pass condition |
|-------|---------------|
| Sub-processor list published? | Yes, accessible and up to date |
| Cloud infrastructure provider named? | Yes (e.g. Azure, AWS, GCP — and region) |
| Notice period for sub-processor changes? | 30 days minimum |
| Right to object to new sub-processors? | Yes, in DPA |

### 2.5 Prompt and Response Logging

| Check | Gap signal |
|-------|-----------|
| Does vendor retain prompts/responses? | If yes: for how long? Is this covered in DPA? |
| Are prompts/responses used for abuse detection? | Generally acceptable; check retention period |
| Is there a retention/deletion policy for vendor-side logs? | Absence = data held indefinitely |

### 2.6 Special Category Data Exposure

| Check | How to identify |
|-------|----------------|
| Does any prompt template include health, political, religious, biometric, or other Art. 9 data? | Review prompt templates, RAG document sets, user-supplied content passed to the model |
| Does the AI infer sensitive attributes? (e.g. sentiment → mental health) | Review model outputs; document inference risk |
| Is explicit consent or Art. 9(2) basis documented for AI processing of special category data? | ROPA + LIA/DPIA |

---

## 3. Standard AI Vendor DPA Reference

When generating or reviewing a DPA with an AI vendor, ensure these AI-specific clauses are present or addressed:

```
AI-SPECIFIC DPA CLAUSES TO VERIFY:

1. TRAINING DATA EXCLUSION
   "The Processor shall not use Controller Personal Data, including prompts and 
   completions, to train, fine-tune, or improve its models or services, unless 
   Controller provides explicit written consent."

2. INFERENCE RESTRICTION
   "The Processor shall not derive or infer special category data (Article 9 GDPR) 
   from Controller Personal Data without explicit instruction."

3. PROMPT/RESPONSE RETENTION
   "Prompts and responses shall be retained for no longer than [X days] for 
   operational purposes and shall be deleted thereafter. No retention for model 
   improvement purposes without consent."

4. MODEL OUTPUTS OWNERSHIP
   "All model outputs generated using Controller Personal Data are the property 
   of the Controller. The Processor retains no licence to use outputs."

5. INCIDENT NOTIFICATION — AI-SPECIFIC
   "In the event of a model output that unexpectedly discloses or infers personal 
   data of third parties, the Processor shall notify the Controller within 24 hours."
```

---

## 4. Training Data Risk

### When is training on personal data a problem?

| Scenario | Risk level | Action |
|----------|-----------|--------|
| Vendor's foundation model trained on public internet data | Low — no personal data flow | No action |
| Customer prompts used to retrain/fine-tune vendor model | High | Opt out; add training exclusion to DPA |
| Company fine-tunes its own model on customer data | High | Requires legal basis; LIA or consent; DPIA likely needed |
| Company uses synthetic data / anonymised data for fine-tuning | Low if genuinely anonymous | Verify anonymisation is robust |
| RAG system indexes personal data from documents | Medium | Treat as normal processing; ROPA entry; access controls |

### Detecting training data use in codebase

```
# OpenAI API — check for training opt-out header or param
Search for: user=, store=false, allow_training

# Anthropic Claude API — check policy compliance
Default: inputs not used for training; verify in account settings

# Google Vertex AI / Gemini — check data governance settings
Search for: dataGovernance, training=false in API config

# Azure OpenAI — data processing agreement governs; generally no training by default
Search for: Azure endpoint (*.openai.azure.com)

# Fine-tuning jobs using personal data
Search for: fine_tuning.jobs.create, FineTuningJob, train_file — check what data is uploaded
```

---

## 5. Art. 22 Automated Decision-Making

AI-driven decisions require Art. 22 compliance when they have legal or similarly significant effects on individuals.

### Trigger assessment

| AI use case | Art. 22 applies? | Action |
|-------------|-----------------|--------|
| Content recommendations | No — informational | Document as profiling; LIA |
| Credit/loan scoring | Yes | Human review, explanation, right to contest |
| Job application screening | Yes | Human review; transparency to candidates |
| Insurance risk pricing | Yes | Human review; transparency |
| Fraud detection leading to account block | Yes | Human review; appeal mechanism |
| Medical diagnosis support | Yes (high risk) | DPIA; clinical oversight |
| Customer churn prediction (internal only) | No | LIA for profiling |
| Ad targeting | Generally no | ePrivacy consent; LIA |

### Art. 22 compliance requirements (if triggered)

- Inform data subjects that automated decision-making is used (Arts. 13/14)
- Provide meaningful information about the logic involved
- Enable data subjects to request human review of any automated decision
- Enable data subjects to contest the decision and express their point of view
- Do not base decisions solely on special category data unless explicit consent (Art. 22(4))

---

## 6. EU AI Act Overlay

The EU AI Act applies from August 2026 (high-risk systems). Identify AI system risk tier from codebase:

| AI use case found | AI Act risk tier | Action required |
|-------------------|-----------------|-----------------|
| Social scoring of individuals | Unacceptable — PROHIBITED | Must not use |
| Real-time biometric identification in public spaces | Unacceptable — PROHIBITED | Must not use |
| CV screening / job ranking | High risk | Conformity assessment; HRMS registration |
| Credit scoring | High risk | Conformity assessment |
| Education/training outcome prediction | High risk | Conformity assessment |
| Biometric categorisation | High risk | DPIA + conformity assessment |
| Customer service chatbot | Limited risk | Transparency obligation: must disclose it's an AI |
| Spam filter, recommendation engine | Minimal risk | No additional obligation |

**Code scan signals for high-risk AI**:
- `resume_parser`, `cv_score`, `candidate_rank` → job screening
- `credit_score`, `risk_model`, `loan_decision` → credit
- `face_recognition`, `emotion_detection`, `biometric` → biometric
- `student_score`, `learning_path` → education

---

## 7. Gap Analysis Additions

Add an **AI Vendor Compliance** section to the gap analysis:

| Check | Severity | Finding | Recommendation |
|-------|----------|---------|----------------|
| AI vendor DPA signed | 🔴 Critical | [finding] | |
| Training data opt-out enabled | 🔴 Critical | [finding] | |
| Data residency documented | 🔴 Critical | [finding] | |
| Transfer mechanism in place (if US vendor) | 🔴 Critical | [finding] | |
| Special category data exposure assessed | 🟠 High | [finding] | |
| Art. 22 assessment for AI decisions | 🟠 High | [finding] | |
| EU AI Act risk tier assessed | 🟠 High | [finding] | |
| Sub-processor list reviewed | 🟡 Medium | [finding] | |
| Prompt/response retention documented | 🟡 Medium | [finding] | |
| AI disclosure in privacy notice | 🟠 High | [finding] | |

---

## 8. Common AI Vendor Status Table

*Verify current status — DPA terms change frequently.*

| Vendor | DPA available | Training opt-out | EU data residency | DPF certified | Transfer mechanism |
|--------|--------------|-----------------|------------------|--------------|-------------------|
| OpenAI (API) | ✅ Yes (via API terms) | ✅ Yes (default for API) | ⚠️ US-based; EU option via Azure OpenAI | ✅ Yes | DPF + SCCs |
| Anthropic (API) | ✅ Yes | ✅ Yes (default for API) | ⚠️ US-based | ✅ Yes | DPF + SCCs |
| Google Vertex AI / Gemini | ✅ Yes (Google Cloud DPA) | ✅ Configurable | ✅ EU regions available | ✅ Yes | DPF + SCCs + Adequacy |
| Azure OpenAI | ✅ Yes (Microsoft DPA) | ✅ Default off | ✅ EU regions | ✅ Yes | DPF + SCCs |
| AWS Bedrock | ✅ Yes (AWS DPA) | ✅ Default off | ✅ EU regions | ✅ Yes | DPF + SCCs |
| Cohere | ✅ Yes | ✅ Enterprise | ⚠️ Check region | ✅ Yes | SCCs |
| Mistral AI | ✅ Yes | ✅ API default | ✅ France-based | N/A (French) | EU controller |
| Hugging Face (Inference API) | ✅ Yes | Check plan | ⚠️ US default | ✅ Yes | SCCs |

*Always verify current status directly with vendor before relying on this table.*
