## Chronotype–Shift Work Misalignment and Cancer Risk (UK Biobank)

**Status**: study overview + analysis plan (code to be released after manuscript is public)  
**Scope**: prospective cohort (UK Biobank RAP) · cancer incidence (overall + site-specific) · chronotype × work schedule framework

---

### What this repository contains

This repository is a **clean, GitHub-friendly record** of the project’s:

- **Research question** and motivation
- **Cohort/QC pipeline** (Part 1)
- **Exposure definitions** (chronotype × shift-work “misalignment” matrix)
- **Planned modelling strategy** (Part 2) and optional **PGS extension** (Part 3)

It intentionally **does not include runnable code or individual-level outputs yet**, to comply with data governance and to align with the planned publication timeline.

---

### Key idea (one sentence)

The same work schedule may not imply the same biological disruption for different people; therefore, **chronotype may modify the health impact of shift work**, and “misalignment” can be more informative than shift work alone.

---

### Exposure framework (reviewer-proof 4-group matrix)

| Group label | Definition | Mechanistic interpretation |
|---|---|---|
| **Aligned (Ref)** | Morning chronotype + Day work | aligned biological & social timing |
| **Social Jetlag** | Evening chronotype + Day work | chronic misalignment without night work |
| **Circadian Disruption** | Morning chronotype + Night work | maximal phase inversion |
| **Environmental Risk** | Evening chronotype + Night work | light-at-night / night-work environment with partial chronotype alignment |

**Shift work without nights** is typically treated as noise for the core mechanistic comparison and is handled via exclusion or sensitivity analyses (project-specific).

---

### Workflow at a glance

| Stage | Purpose | Typical output (conceptual) |
|---|---|---|
| **Part 1 (QC)** | build lag-adjusted incident cancer cohort + robust covariate coding | `baseline_qc_lag2.csv` (analysis-ready cohort) |
| **Part 1.5 (scan)** | exploratory site scan + group-definition validation | incidence tables + quick age/sex Cox scans |
| **Part 2 (models)** | publication-grade modelling (Cox, PH checks, sensitivity, interactions) | manuscript-ready tables/figures |
| **Part 3 (PGS, optional)** | chronotype PGS for genotype–phenotype stratification & G×E | per-participant PGS + stratification outputs |

---

### Data governance and release plan

- UK Biobank data are access-controlled and **cannot be redistributed** here.
- This repository avoids posting individual-level outputs or restricted derivatives.
- **After the manuscript becomes public**, I plan to upload:
  - cleaned notebooks/scripts (RAP-ready)
  - a minimal config/path template
  - a “required UKB fields” checklist

---

### Read next

- **Full study document**: `STUDY_OVERVIEW.md`  

---

### Contact / collaboration

If you plan to reuse the study design or collaborate, please treat this repository as a protocol/analysis-plan record and coordinate authorship accordingly.

