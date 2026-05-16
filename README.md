# OpenPulse

**Intern:** Kening Li
**Project Type:** Computation Engine

## Overview
OpenPulse is an AI-enabled signal detection engine for real-world evidence. The project uses a Streamlit form to collect drug-event cohorts, computes disproportionality metrics, and cross-validates findings against label and secondary evidence sources.

## Deliverable
- Streamlit report form for drug, event, and time-range inputs
- Signal-detection report with PRR/ROR metrics and label cross-checks
- Downloadable report package with YAML cohort metadata

## Core Tools
- openFDA FAERS
- ClinicalTrials.gov
- openFDA Drugs@FDA
- DailyMed
- SIDER
- PubChem / MedDRA

## Tech Stack
Python, Streamlit, `google-generativeai`, pandas, scipy, PyYAML

## Notes
This project focuses on reproducible safety signal reports rather than a conversational agent.
