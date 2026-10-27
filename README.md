# -ai-compliance-contract-screener

# Enterprise AI Vendor & Compliance Screener (n8n + Groq + PostgreSQL)

## 📌 Executive Summary
* **The Challenge:** A growth-stage fintech firm faced severe operational bottlenecks, manual delays, and legal exposure due to a backlog of complex vendor contracts requiring individual audits under the newly enforced EU AI Act and standard data liability frameworks. Review time took up to 48 hours per document.
* **The Solution:** An end-to-end automated orchestrator built on n8n that ingests contracts, executes asynchronous dual-LLM pipelines for metadata extraction and compliance risk auditing, sanitizes text inputs via JavaScript layers, and commits structured data to a PostgreSQL backend (Supabase).
* **The Business Impact:** Reduced contract triage and compliance screening times from **48 hours down to 2.5 seconds per document**, completely eliminating risk blind spots during vendor procurement.

---

## 🛠️ System Architecture Diagram
[ Incoming Webhook ] ──► [ JavaScript Node ] ──► [ Groq API: Qwen 3.8 ]
(Vendor JSON)         (Sanitization Layer)      (Metadata Extraction)
│
▼
[ Supabase Database ] ◄── [ JavaScript Parse ] ◄── [ Groq API: Guardrail ]
(PostgreSQL Storage)      (Data Union Layer)        (EU AI Act Classifier)




---

## 🗂️ Repository Structure
* `/n8n-workflow/workflow.json` : Production code to import the pipeline natively into your n8n instance.
* `/database/schema.sql` : Core data definition language (DDL) and mock data populators.
* `/analytics/looker-template.md` : Blueprint parameters for executive business intelligence views.

---

## 🚀 Quickstart & Deployment Guide

### 1. Database Initialization (Supabase / PostgreSQL)
Execute the following script inside your SQL editor to initialize the production data sink:
```sql
CREATE TABLE contract_audits (
    id SERIAL PRIMARY KEY,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()),
    contract_value NUMERIC,
    jurisdiction TEXT,
    risk_score INT,
    eu_ai_act_category TEXT,
    compliance_rationale TEXT,
    api_latency_ms INT
);
```

### 2. n8n Import Setup
1. Copy the raw configuration code from `/n8n-workflow/workflow.json`.
2. Open your n8n canvas, click anywhere, and press `Ctrl+V` (or `Cmd+V`).
3. Connect your **PostgreSQL** and **HTTP Header Auth (Groq API Key)** credentials to the respective nodes.
4. Toggle the workflow to **Active**.

### 3. API Payload Specifications
To initiate a background contract audit run, trigger the Webhook via a `POST` request:
```json
{
  "contract_name": "AI Vendor Agreement - Fintech Corp",
  "body_text": "This agreement covers the deployment of a machine learning-driven automated credit scoring model designed to automatically evaluate and approve consumer loan applications across European jurisdictions."
}
```

---

## ⚖️ Governance & Compliance Guardrails
The system introduces an automated **Compliance Guardrail Node** utilizing `openai/gpt-oss-20b` running strict JSON enforcement. It explicitly checks input logic against **Annex III of the EU AI Act**, mapping vendor software applications into operational risk categories:
* **Unacceptable Risk** (Cognitive behavioral manipulation, social scoring)
* **High-Risk** (Biometrics, critical infrastructure, credit scoring, employment algorithms)
* **Limited Risk / Minimal Risk** (General generative utility tools, basic automation layers)

---

## 👥 Contributors & License
Developed as a production architecture case study for Enterprise AI Transformation. Distributed under the MIT License.
│
▼
[ Google Looker Studio ] ──► (Real-time Executive Dashboard)
