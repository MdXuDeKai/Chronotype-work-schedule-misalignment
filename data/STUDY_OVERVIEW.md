# Chronotype–Shift Work Misalignment and Cancer Risk (UK Biobank)

This repository documents the **study rationale, cohort pipeline, exposure framework, and analysis plan** for a UK Biobank project investigating whether **circadian misalignment**—operationalised through **chronotype × shift-work patterns**—is associated with **overall and site-specific cancer incidence**, and how the interpretation changes after rigorous control of confounding and reference-group contamination.

> **Code & individual-level data are intentionally not included at this stage.**  
> I plan to upload runnable notebooks/scripts **after the manuscript becomes public**.  
> This repository currently serves as a **methods- and study-design record** suitable for collaboration and transparency.

---

## Project snapshot

- **Data source**: UK Biobank (UKB) on the Research Analysis Platform (RAP)
- **Design**: prospective cohort
- **Primary outcome**: incident **any cancer** excluding non-melanoma skin cancer (ICD-10 C44)
- **Key site-specific outcomes** (pre-defined): lung, breast, colorectal, prostate, upper GI, liver/biliary, pancreas, kidney, bladder, melanoma, NHL, leukaemia, brain/CNS, thyroid, ovary, uterus, head & neck (availability depends on the QC output)
- **Core exposure concept**: **chronotype–work schedule alignment vs misalignment**

---

## Scientific motivation

Night shift work has been extensively studied, but results can be inconsistent across outcomes and populations. A major reason is that **the same work schedule may not imply the same biological disruption for different people**.

This project focuses on the hypothesis that:

1. **Chronotype is an effect modifier** of shift work–related health risks.
2. A “one-size-fits-all” night-shift exposure definition misses biologically meaningful heterogeneity.
3. For cancer epidemiology, inference is sensitive to:
   - **smoking confounding** (especially for lung cancer),
   - **healthy worker selection / survivor bias**, and
   - **reference-group contamination** (e.g., day workers who previously worked nights).

---

## Data pipeline and analysis stages

### Part 1 — Baseline QC cohort construction (v3.1)

Goal: generate a **clean, lag-adjusted incident cancer cohort** (1 row per participant) with consistent outcome and covariate definitions.

Key features confirmed in the current running notebook (`Part1_QC_Baseline_Cohort_v31（运行版）.ipynb`):

- **Eligibility / QC**:
  - remove participants with essential missing baseline information
  - exclude baseline cancer (with special handling for C44)
  - apply a **2-year lag** window to reduce reverse causation
- **Outcomes**:
  - any cancer (excluding C44) and multiple site-specific outcomes
  - outcome-specific follow-up time variables (cause-specific hazard framework)
- **Shift work classification**:
  - current shift-work and night-shift variables (UKB fields such as p826, p3426)
- **Employment history upgrade (critical)**:
  - uses **employment history night-shift-specific fields** (e.g., p22640/p22650) to identify **ever night shift history**
  - derives **cumulative night shift duration** using employment history duration fields (e.g., p22641/p22651) for dose–response analyses
- **Covariate integrity fixes**:
  - derives consistent **ethnicity** and **education** indicators to avoid UKB hierarchical coding pitfalls (e.g., White subcodes; “none of the above” vs missing)

Main output (conceptual): a QC’d cohort table (commonly referred to in the project as `baseline_qc_lag2.csv`) that downstream parts read as the single source of truth.

### Part 1.5 — Exploratory site scan + “Reviewer-proof” grouping

Goal: before running publication-grade multivariable models, perform:

- **site-specific crude incidence** summaries by group
- fast **Model 1 Cox** (age + sex) across many cancer sites
- validation of a clearer, mechanism-aligned risk grouping (see below)
- optional sensitivity checks related to “clean” reference definitions

This stage is described in `Part1_5_代码分析与说明.md` and produces tables/figures for *signal screening*, not final causal claims.

### Part 2 — Formal modelling (publication-grade pipeline)

Goal: run a full analysis pipeline (descriptive, KM curves, Cox models, PH checks, subgroup/interaction, sensitivity analyses, E-values), aligned with a Q1/SLEEP-level expectation.

The document `Part2_V2_版本对比与优势总结.md` summarises how a modular Part 2 implementation improves over earlier iterations (e.g., adding PH diagnostics, site-specific analyses, and structured sensitivity analyses).

### Part 3 — Polygenic score (PGS) extension (optional mechanistic/stratification add-on)

Goal: integrate a **chronotype PGS** to probe:

- **genotype–phenotype concordance/discordance** (“innate vs acquired” eveningness proxy)
- potential **G×E**: whether genetic liability to eveningness modifies night work–related risk
- risk stratification (e.g., ER group × PGS quantiles)

This is a planned extension and not required for the core epidemiologic results.

---

## Exposure framework: chronotype × work schedule

### “Reviewer-proof” 4-group matrix (used in Part 1.5 and intended for Part 2 alignment)

This project converged on a mechanistically interpretable 4-group framework:

- **Aligned (reference)**: Morning chronotype + Day work  
  Concept: biological and social timing aligned.

- **Social Jetlag**: Evening chronotype + Day work  
  Concept: chronic misalignment without night work; “social time vs biological time”.

- **Circadian Disruption**: Morning chronotype + Night work (some/permanent nights)  
  Concept: maximal phase inversion; strongest misalignment hypothesis.

- **Environmental Risk**: Evening chronotype + Night work (some/permanent nights)  
  Concept: light-at-night and night-work environment, but with partial chronotype alignment.

**Shift work with no nights (“shift_no_night”)** is often treated as noise for the mechanistic story and can be excluded from the main 4-group comparison (or analysed separately as a sensitivity analysis).

---

## Reference group contamination and “clean reference” logic

A recurring methodological issue in shift-work studies is that **current day workers may include former night workers**. If such participants are kept in the reference group, effect estimates may be biased.

This project’s QC pipeline explicitly supports constructing a stricter reference definition using employment history night-shift fields (not generic shift-work history fields), enabling:

- **primary analysis** using an operational reference group definition
- **sensitivity analysis** using a cleaner, verified “never night shift” reference group where feasible

---

## Confounding control strategy (high-level)

The project is built around transparent model escalation rather than “one fully adjusted model”.

Key confounding domains include:

- **Demographics**: age, sex
- **Socioeconomic**: deprivation index, education, income, ethnicity
- **Lifestyle**: smoking (critical), alcohol, BMI, physical activity
- **Comorbidity / baseline health**: comorbidity indices/flags (subject to data availability)
- **Family history**: cancer family history indicators where available

Special note: for lung cancer, the project explicitly treats **smoking** as the dominant confounder and prioritises:

- audit of smoking distributions across groups,
- stable sample definitions across models to avoid “sample drift” artefacts,
- sensitivity analyses that address residual confounding concerns.

---

## Outputs (high-level)

The project’s workflow is designed so that each part has a clear output “contract”:

- **Part 1**: a single analysis-ready cohort table (QC + outcomes + covariates)
- **Part 1.5**: exploratory tables/figures + an enriched cohort export with group labels (project-specific naming)
- **Part 2**: manuscript-ready Tables/Figures + supplementary tables
- **Part 3**: per-participant PGS values and derived stratification variables (optional)

---

## Reproducibility and data governance

- UK Biobank data are access-controlled and cannot be redistributed here.
- This repository intentionally avoids publishing any individual-level data or UKB-derived restricted outputs.
- After manuscript public release, I plan to publish:
  - a cleaned, executable version of the notebooks/scripts,
  - a minimal “paths/config” template for RAP execution,
  - a list of required UKB fields and expected derived columns.

---

## Suggested citation / authorship note

If you use this repository’s design/materials for collaboration, please cite it as a study protocol/analysis plan and coordinate authorship according to the project agreement.

