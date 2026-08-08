Haan — **ye README kaafi strong hai**, lekin abhi main ise final nahi maanunga. Isme kuch claims aise hain jo actual implementation se **prove hone chahiye**, warna judges ke saamne risk hoga.

Challenge brief ke mutabiq judges especially **AI/data quality, working product, safety/reliability, evaluation aur pitch clarity** dekh rahe hain. 

### Mere hisaab se 5 important improvements:

1. **“Production-grade / enterprise-grade”** ko thoda controlled rakho. Hackathon prototype hai, actual production system nahi.
2. `app.py` ko huge single-file architecture na banao — hum actual project ko modular rakhenge.
3. **Mock case pack** ko clearly `development/demo data` label karo. Actual organizer case pack aane par replace hoga.
4. Evaluation numbers **fake/hardcoded nahi hone chahiye**; actual held-out cases se calculate honge. Brief explicitly reference/held-out evaluation maangta hai. 
5. README mein **installation, architecture, decision flow, evidence model, limitations, demo cases aur evaluation methodology** clearly honi chahiye.

---

# 🏆 Main tumhare README ko is version mein convert karunga

Isko **abhi GitHub README mein paste kar sakte ho**, lekin ek important baat: jahan implementation abhi bani nahi hai, wahan hum false claims nahi karenge.

````markdown
# 🏭 SOURCEX AI
## Evidence-Grounded Manufacturing Decision Copilot

### Sofstica SGTDP 2026 Hackathon
**Primary Track:** Track 1 — Supplier Shortlisting

---

## 🚀 Overview

SOURCEX AI is an evidence-grounded manufacturing decision-support prototype designed to help product, sourcing, procurement, and manufacturing-operations teams evaluate and shortlist suppliers from a structured challenge pack.

Instead of producing an unexplained AI recommendation, the system follows a transparent decision pipeline:

**Requirements → Evidence → Eligibility → Ranking → Sensitivity Analysis → Recommendation**

The system is designed to make manufacturing sourcing decisions:

- Faster
- Traceable
- Evidence-grounded
- Explainable
- Robust to missing or conflicting information

> **Important:** SOURCEX AI is a decision-support system. It does not approve suppliers, contact suppliers, issue RFQs, negotiate commercially, or place orders.

---

# 🎯 Problem

Manufacturing sourcing decisions often require teams to reconcile information across:

- Product requirements
- Bills of materials / component specifications
- Supplier profiles
- Manufacturing capabilities
- Certifications
- Quality history
- Minimum order quantities
- Lead times
- Sustainability evidence
- Commercial assumptions

This creates a time-consuming process and makes it difficult to explain why one supplier was preferred over another.

SOURCEX AI addresses this by converting the supplied evidence into a structured and traceable supplier decision.

---

# 💡 Solution

The system performs two major stages.

### Stage 1 — Eligibility Screening

Suppliers are evaluated against mandatory requirements before ranking.

Example:

| Requirement | Supplier A | Supplier B |
|---|---:|---:|
| Manufacturing Capability | ✅ PASS | ✅ PASS |
| Certification | ✅ PASS | ❌ FAIL |
| MOQ | ✅ PASS | ✅ PASS |
| Lead Time | ✅ PASS | ✅ PASS |
| Evidence Availability | ✅ | ⚠️ |

A supplier that fails a mandatory requirement is not treated as an ordinary ranked candidate.

---

### Stage 2 — Supplier Ranking

Eligible suppliers are evaluated using configurable decision priorities.

Example dimensions:

- Manufacturing capability
- Quality
- Certification
- Lead time
- Cost
- Capacity
- Sustainability

The ranking weights can be changed to evaluate how sensitive the recommendation is to different priorities.

---

# 🔎 Evidence-Grounded Decisions

Every material supplier claim should be traceable to its source.

Example:

```text
Supplier: Supplier A

Certification:
ISO 9001

Evidence:
supplier_profile.pdf
Page 4

Status:
Verified from supplied evidence
````

The system distinguishes between:

### FACT

Information directly supported by the challenge-pack evidence.

### ASSUMPTION

A value or interpretation introduced by the system where explicitly permitted.

### RECOMMENDATION

A system-generated conclusion derived from the verified facts and configured decision criteria.

This separation prevents recommendations from being presented as source facts.

---

# ⚠️ Missing & Conflicting Information

SOURCEX AI does not silently guess when evidence is incomplete or contradictory.

Example:

```text
Supplier A

Lead Time — Source 1:
15 days

Lead Time — Source 2:
28 days

Status:
⚠️ CONFLICT

Action:
Human review required
```

Similarly, if mandatory certification evidence is unavailable:

```text
Certification:
UNKNOWN

Status:
⚠️ INSUFFICIENT EVIDENCE

Recommendation:
Do not treat certification as verified
```

---

# 📊 Sensitivity Analysis

A supplier recommendation can change when business priorities change.

SOURCEX AI allows users to evaluate scenarios such as:

### Balanced

```text
Cost        20%
Quality     25%
Lead Time   20%
Capability  25%
Other       10%
```

### Cost-First

```text
Cost        40%
Quality     20%
Lead Time   15%
Capability  15%
Other       10%
```

### Speed-First

```text
Cost        15%
Quality     20%
Lead Time   40%
Capability  15%
Other       10%
```

The system recomputes supplier rankings under each scenario and highlights recommendation changes.

---

# 🧠 Decision Trace

For every recommendation, the system should be able to explain:

```text
Product Requirement
        ↓
Mandatory Constraint
        ↓
Supplier Evidence
        ↓
Constraint Result
        ↓
Scoring
        ↓
Supplier Ranking
        ↓
Recommendation
```

This creates a traceable path from source evidence to final recommendation.

---

# 🏗️ Architecture

```text
                  User
                   │
                   ▼
          ┌─────────────────┐
          │ Streamlit UI    │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ Data Ingestion  │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ Evidence Layer  │
          └────────┬────────┘
                   │
          ┌────────┴─────────┐
          ▼                  ▼
   Constraint Engine   Evidence Retrieval
          │                  │
          └────────┬─────────┘
                   ▼
          ┌─────────────────┐
          │ Ranking Engine  │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ Sensitivity     │
          │ Analysis        │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ Decision Report │
          └─────────────────┘
```

---

# 📁 Project Structure

```text
sourcEX-ai/
│
├── app/
│   └── app.py
│
├── ai/
│   ├── extraction.py
│   ├── retrieval.py
│   └── explanation.py
│
├── engine/
│   ├── constraints.py
│   ├── scoring.py
│   ├── sensitivity.py
│   ├── conflicts.py
│   └── confidence.py
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── sample/
│
├── evaluation/
│   ├── benchmark.py
│   └── metrics.py
│
├── workflows/
│   └── n8n/
│
├── tests/
│
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

---

# 🛠️ Technology Stack

### Frontend

* Streamlit

### Core Engine

* Python
* Pandas
* Pydantic

### AI Layer

* Document understanding
* Evidence retrieval
* Structured AI outputs
* Recommendation explanation

### Automation

* n8n

### Evaluation

* Python-based benchmark and metrics

---

# 📦 Running Locally

## 1. Clone repository

```bash
git clone <REPOSITORY_URL>
cd sourcEX-ai
```

## 2. Create virtual environment

```bash
python -m venv .venv
```

### Windows

```bash
.venv\Scripts\activate
```

### macOS / Linux

```bash
source .venv/bin/activate
```

## 3. Install dependencies

```bash
pip install -r requirements.txt
```

## 4. Run Streamlit

```bash
streamlit run app/app.py
```

The application will normally be available at:

```text
http://localhost:8501
```

---

# 🧪 Evaluation

The system will be evaluated against organizer-provided held-out cases or reference calculations.

Key metrics include:

* Mandatory-constraint satisfaction rate
* Evidence citation coverage
* Citation correctness
* Unsupported-claim / hallucination rate
* Recommendation agreement
* Cost / lead-time error where applicable
* Completion time
* Human-review effort
* Robustness to missing or conflicting inputs

Evaluation results will be generated from the actual benchmark cases rather than manually assigned scores.

---

# 🧩 Required Demonstration Cases

The prototype will demonstrate:

### 1. Successful Case

A supplier satisfies the mandatory requirements and receives a defensible recommendation.

### 2. Ambiguous / Conflicting Case

The system detects contradictory or incomplete evidence and surfaces the issue.

### 3. Failure / Fallback Case

The system cannot safely complete the decision and escalates to human review.

---

# 🔐 Safety & Governance

SOURCEX AI operates strictly as a decision-support system.

The prototype does not:

* Contact real suppliers
* Send RFQs
* Approve vendors
* Place purchase orders
* Negotiate commercial terms
* Present inferred information as verified evidence

Consequential actions remain under explicit human control.

---

# 🛡️ Data & Security Principles

* Challenge-pack evidence is treated as untrusted input.
* Source evidence is separated from model-generated recommendations.
* API credentials are never committed to the repository.
* Confidential case materials are not uploaded to unapproved external services.
* Missing or conflicting evidence is surfaced instead of silently guessed.
* External information, if permitted by event rules, must be disclosed with its source and retrieval information.

---

# 📈 Baseline Comparison

The system will compare the automated workflow against a stated manual baseline.

Example evaluation structure:

```text
Metric                  Manual       SOURCEX AI
------------------------------------------------
Completion Time         TBD          TBD
Review Effort           TBD          TBD
Constraint Accuracy     TBD          TBD
Citation Coverage       N/A          TBD
Recommendation Match    TBD          TBD
```

Final values will be populated from actual evaluation results.

---

# 🚀 Future Roadmap

### Phase 1

Organizer challenge-pack ingestion.

### Phase 2

Advanced document extraction and evidence indexing.

### Phase 3

Improved conflict and uncertainty detection.

### Phase 4

Expanded quotation and landed-cost analysis.

### Phase 5

Supply-risk scenario planning.

These extensions remain subject to challenge rules and evaluation requirements.

---

# 👥 Intended Users

Primary users:

* Product teams
* Sourcing teams
* Procurement teams
* Manufacturing-operations teams

The system is intended to support—not replace—human decision makers.

---

# ⚠️ Limitations

SOURCEX AI does not independently verify:

* Real-time supplier availability
* Actual production capacity
* Current market pricing
* Legal or regulatory compliance
* Customs requirements
* Engineering certification validity

The system only reasons over the evidence available to it and clearly identifies gaps where evidence is insufficient.

---

# 🏆 Hackathon Alignment

SOURCEX AI is designed around the requirements of:

**Sofstica SGTDP 2026 — Manufacturing Decision Copilot**

Primary Track:

**Track 1 — Supplier Shortlisting**

The system focuses on:

**Eligibility → Evidence → Ranking → Sensitivity → Explanation → Human Review**

---

## 📜 License

Add the appropriate license before final submission.

---

## ⚠️ Hackathon Prototype

This repository contains a prototype developed for hackathon evaluation.

It is not intended to independently authorize manufacturing, procurement, supplier approval, or commercial transactions.

```

## 🔥 Lekin ek important correction

Tumhari current README mein ye line:

> **“Cryptographically verified mock snapshot (`SGTDP-MFG-CASEPACK-v1.2`) embedded directly…”**

**abhi mat rakho**, jab tak hum actual mock snapshot, SHA-256 checksum aur verification code bana nahi dete.

Similarly:

> “6 candidate suppliers”

> “5 mandatory constraints”

> “SUP-004”

> “SUP-005”

ye sab **sirf tab final README mein rakhenge jab hamare actual development dataset mein waqai ye hon**.

Challenge brief mein organizers ke case pack ke andar **data dictionary, source manifest aur SHA-256 checksums** hone ka mention hai, lekin tumhare uploaded brief mein `SGTDP-MFG-CASEPACK-v1.2`, SUP-004, SUP-005 ya exact six suppliers ka evidence nahi hai. :contentReference[oaicite:2]{index=2}

**Isliye winner banne ke chakkar mein README mein unsupported claims nahi likhenge.** Judges code dekh sakte hain.

### Abhi kya karo?

**Is improved README ko GitHub mein save karo.**

Phir hum actual coding start karenge:

> **STEP 2 → Sample Challenge Pack + Requirements/Supplier data model**

Aur us step mein main tumhe **exact files, exact folders aur exact code** dunga. Tum bas copy/paste karke run karoge, aur hum ek ek module test karte jayenge.
```
