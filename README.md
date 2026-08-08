# AI Manufacturing Decision Copilot
**Sofstica SGTDP 2026 Hackathon — Track 1: Supplier Shortlisting**

A single-file Streamlit application that converts a manufacturing challenge pack
(product requirements + supplier profiles + quotations) into a transparent,
evidence-grounded, ranked shortlist of eligible suppliers.

---

## 🚀 Run it (takes ~1 minute)

```bash
pip install -r requirements.txt
streamlit run app.py
```

Then open the URL Streamlit prints (usually `http://localhost:8501`).

No API keys, no database, no external services required — the entire
challenge pack is embedded as structured mock data inside `app.py`, so it
runs instantly and identically on any machine.

**Requires:** Python 3.9+

---

## 🎯 What it does (Track 1 requirements → where they live)

| Brief requirement | Where it's implemented |
|---|---|
| Transparent eligibility screen before ranking | `screen_supplier()` — every supplier checked against 5 mandatory constraints independently, each with a pass/fail/gap verdict |
| Source citations for every material supplier claim | Every fact card and constraint check row shows a `source_doc` (e.g. `supplier_profile_apex.pdf`) |
| Sensitivity analysis showing ranking changes | "Sensitivity Analysis" section recomputes ranking under Cost-first / Speed-first / Quality-first / Your-weights profiles |
| Handling ambiguities & conflicts | SUP-004 (missing certification data) and SUP-005 (conflicting lead-time figures between two source docs) demonstrate explicit flagging, not silent resolution |
| Safety & human control notice | Persistent banner + sidebar lock toggle stating decision support only — no live supplier contact/ordering capability exists anywhere in the code |
| Evaluation metrics dashboard | Constraint satisfaction rate, citation coverage, unsupported-claim rate, and agreement vs. a reference held-out evaluation set |
| Facts vs. assumptions vs. recommendations | Dedicated section + inline tags separating extracted facts, tool-introduced assumptions (e.g. backfilled sustainability score), and model-generated ranking |
| One successful / one ambiguous / one failure case | "Required Demonstration Cases" tabbed section |
| Intended user, assumptions, limitations, human-approval points | Bottom section of the app |

## 🧱 Architecture

```
app.py
├── Mock Challenge Pack        (PRODUCT_BRIEF, SUPPLIERS, REFERENCE_EVALUATION)
├── Eligibility Engine         (screen_supplier, ConstraintCheck)
├── Ranking Engine             (compute_ranking_score)
├── Evaluation Engine          (compute_evaluation_metrics, checksum)
├── Theming                    (inject_css — dark/light industrial dashboard)
├── UI components               (sidebar, header, cards, checks, charts)
└── main()                     wires everything into the Streamlit page
```

**Data flow:** sidebar inputs → `screen_supplier()` runs 5 independent
constraint checks per supplier (capability, ISO cert, MOQ, lead time,
quality) → eligible/conditional suppliers go to `compute_ranking_score()`
for a weighted composite score → results render as cards, a bar chart, a
sensitivity table, and an evaluation dashboard.

## 🔒 Safety boundary

This prototype **cannot** contact suppliers, send requests for quotation,
approve vendors, or place orders — no such network calls or write actions
exist anywhere in the code. The sidebar "Human Control Lock" makes this
boundary visible in the UI, matching the challenge brief's submission
boundary and safety requirements.

## 🔧 What I'd improve with more time

- Replace the embedded mock pack with a real loader for the organizer's
  versioned case-pack files (PDF/CSV parsing) and verify SHA-256 checksums
  against their manifest before screening.
- Add an LLM-based extraction layer to pull constraint values out of
  unstructured supplier PDFs automatically, with the same citation
  discipline currently hardcoded into the mock data.
- Persist screening runs so judges can diff two requirement configurations
  side by side instead of re-running the sidebar manually.
- Expand conflict detection beyond the two seeded cases into a generic
  cross-document consistency checker.
