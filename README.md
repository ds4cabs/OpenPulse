# OpenPulse

[![CABS: ds4cabs](https://img.shields.io/badge/CABS-ds4cabs-1f4b99?logo=github)](https://github.com/ds4cabs)
[![GitHub Pages: live](https://img.shields.io/badge/GitHub_Pages-live-brightgreen?logo=github)](https://ds4cabs.github.io/OpenPulse/)
![CABS: 2026](https://img.shields.io/badge/CABS-2026-6f42c1)
![status: MVP in progress](https://img.shields.io/badge/status-MVP_in_progress-f1c40f)
![type: Computation Engine](https://img.shields.io/badge/type-Computation_Engine-1f6feb)
![domain: Pharmacovigilance](https://img.shields.io/badge/domain-Pharmacovigilance-0aa)

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
