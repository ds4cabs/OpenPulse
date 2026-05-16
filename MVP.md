# OpenPulse — MVP Version (Gemini AI Agent · Computation Engine)

**Intern:** Kening Li
**Level:** Master student (UCSD + Weill Cornell), RWE / Biostatistics background
**Timeline:** 2–3 weeks (~52 hours)
**Paradigm:** **Computation Engine** — Streamlit form triggering autonomous Gemini workflow producing a reproducible signal-detection report.
**Database count:** **6** (expanded from 4 to add DailyMed for label cross-check and SIDER for alternative side-effect evidence).

---

## The Agent

**What the agent does (autonomous workflow on submit):** Agent autonomously pulls FAERS records, computes PRR + ROR with CIs, cross-references the drug label (DailyMed) and the SIDER side-effect catalog (different evidence type than spontaneous reporting), looks up trial-population context, normalizes the event term — and writes a signal-detection report with versioned cohort YAML.

**Input:** Form — drug, event, year range, cohort filters.
**Output:** Signal-detection report (Markdown + zip with cohort YAML + raw data).

**Tools (6 public databases):**

1. `get_faers_records(drug_name, year_range, cohort_filters)` — **openFDA FAERS**.
2. `get_trial_population(drug_name)` — **ClinicalTrials.gov**.
3. `get_drug_label(drug_name)` — **openFDA Drugs@FDA**: basic label metadata.
4. `lookup_meddra_synonyms(event_term)` — **PubChem / MedDRA**.
5. `get_dailymed_full_label(drug_name)` — **DailyMed API**: full structured label (richer than Drugs@FDA — for adverse-reaction tables, contraindications, post-marketing experience sections).
6. `get_sider_side_effects(drug_name)` — **SIDER**: side effects extracted from drug labels (label-derived evidence as a cross-check on FAERS spontaneous reporting).

Plus computation utility: `compute_disproportionality(records_df, event)` returning PRR + ROR + 95% CIs.

**Example runs (≥3):**

- *Form:* drug=empagliflozin, event=DKA, year=2022-2024. *Output:* PRR=4.2 (95% CI 3.1-5.7), DailyMed boxed-warning cross-check, SIDER comparison (is DKA listed on the label?), cohort YAML.
- *Form:* drug=dapagliflozin, event="urinary tract infection". *Output:* signal with disproportionality + label cross-reference.
- *Form:* drug=canagliflozin, event="lower limb amputation". *Output:* validates known historical signal; SIDER lookup confirms label inclusion year.

---

## Week-by-Week

**Week 1 (~16h):** Build 6 tool functions + computation utility. Validate SGLT2/DKA against published reference.
**Week 2 (~22h):** Clone computation-engine sub-template. Streamlit form + Gemini agent.
**Week 3 (~14h):** Run 5 analyses; validate reproducibility; README + demo.

## What's OUT

WHO ICTRP (Reuben has), CMS NADAC, Medicaid Drug Pricing, SEC EDGAR; propensity matching, survival analysis (lifelines), full PyMC stats; MONDO/MeSH harmonization beyond MedDRA; onboarding-tutor mode.

## Stretch Goals

- 7th tool: `get_pubmed_evidence(drug, event)` for literature corroboration.

## Realistic CV Entry

*Built OpenPulse, a working Gemini AI computation-engine agent for real-world evidence signal detection integrating 6 public databases.*

- Wrapped 6 public databases (openFDA FAERS, ClinicalTrials.gov, openFDA Drugs@FDA, PubChem/MedDRA, DailyMed, SIDER) plus a PRR/ROR computation utility into a Gemini agent.
- Cross-referenced spontaneous-reporting signals (FAERS) against label-derived evidence (SIDER + DailyMed) for every detected signal, surfacing label-novelty cases.

## Tech Stack

Python, `google-generativeai`, Streamlit, pandas, scipy.stats, PyYAML, matplotlib, openFDA FAERS, ClinicalTrials.gov, openFDA Drugs@FDA, PubChem, DailyMed API, SIDER.

---

## Shared Agent Skeleton (three paradigms, one Gemini primitive)

Every intern's agent uses Gemini's automatic function calling, but the interface layer differs by paradigm. The cohort uses **one starter repo with three sub-templates** that interns clone in week 1:

- **Dossier-generator template** — CLI script: takes structured args, runs the agent workflow autonomously, writes `*.md` + `*.json` to disk. Used by Beyza, Chin Hung, Christina, Shucheng, Xiaoxue.
- **Dashboard template** — Streamlit page with selectors and tables; the agent is invoked on button-click for specific synthesis tasks. Used by Aaron, Jason, Shawn.
- **Computation-engine template** — Streamlit form (or CLI) that takes structured analytical inputs, runs the agent workflow, produces a downloadable analytical report with plots. Used by Reuben, Kening, Natalie.

**Why no chat interfaces?** Scientists need reproducible, shareable artifacts. The agent dimension (Gemini-as-orchestrator, autonomous tool-calling across multiple public databases, synthesis across sources) is preserved in all three paradigms; only the deliverable shape changes.

**Christina** (OpenRepurpose evidence-and-validation module) owns the starter repo with all three sub-templates. The shared repo should also include pre-built wrappers for the most heavily-used databases (ChEMBL, openFDA FAERS, Open Targets, ClinVar) so multiple interns don't redo the same boilerplate.

### Reference snippet — Gemini function calling (same across all three paradigms)

```python
import google.generativeai as genai
import os
genai.configure(api_key=os.environ["GEMINI_API_KEY"])

def my_tool(arg: str) -> dict:
    """One-line docstring Gemini uses to decide when to call this tool."""
    return {"result": ...}

model = genai.GenerativeModel(
    model_name="gemini-2.5-flash",
    tools=[my_tool, other_tool, ...],   # 4-8 tools per agent
    system_instruction=open("system_prompt.md").read(),
)
chat = model.start_chat(enable_automatic_function_calling=True)
response = chat.send_message("structured request — one shot, not a conversation")
```
