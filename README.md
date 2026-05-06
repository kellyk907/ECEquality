# ECE Classroom Quality Dashboard — ECERS-3, CLASS & ASQ-3 Analysis

**Tools:** R (lm, ggplot2) · SPSS v28 · Power BI · ECERS-3 · CLASS · ASQ-3 · TS GOLD  
**Role simulated:** Early Childhood Data Analyst  
**Target org type:** Multi-site ECE provider with QI infrastructure  
**Live dashboard:** * ecequality.netlify.app *

---

## Project overview

This project demonstrates the kind of classroom-level quality analysis a data analyst would support at a large ECE organization like KinderCare. Using a practitioner-derived synthetic dataset of 7 classrooms and 133 children (ages 3–5), I built an interactive analytics dashboard and ran statistical analysis to answer three questions that matter in real ECE program management:

1. Do ECERS-3 structural scores accurately reflect overall program quality — or are they masking something else?
2. What child-level factors are actually associated with attendance, and which ones are within the program's control?
3. Where should improvement resources be directed first, given real constraints on staffing and facilities?

The dataset was built to reflect realistic program conditions, including missing observations, children with inconsistent attendance patterns that don't fit clean predictors, and a classroom carrying a disproportionate IEP load with a vacant co-teacher position. Real program data is messy. This one is too.

---

## Dataset

**Sheet 1 — Classroom Observations** (7 rows, one per classroom)
ECERS-3 subscale scores across all 6 subscales, CLASS domain scores (Emotional Support, Classroom Organization, Instructional Support), handwashing compliance percentage, attendance rate, IEP/IFSP count, DLL count, and observer notes. One observation period per classroom, October 2024.

**Sheet 2 — Child Records** (133 rows, one per child)
Age in months, enrollment type (FT/PT), subsidy status, IEP/IFSP flag, DLL flag, primary language, attendance rate, absences YTD, ASQ-3 scores across all five developmental domains, handwashing duration observed in seconds, pass/fail flag, and family engagement score. Missing values present by design — CR04 handwashing data is incomplete (observer interrupted mid-observation), and family engagement scores are missing for approximately 12% of children.

**Data note:** Practitioner-derived synthetic dataset. Numbers reflect realistic ECE program patterns drawn from direct field experience with ECERS-3, CLASS, ASQ-3, and TS GOLD assessment systems. This is not real child data.

---

## Tools & methods

### R — linear regression (attendance predictors)

A linear regression was run using R's `lm()` function to identify which child-level factors predicted attendance rate across all 133 children.

**Model:** `attendance_rate ~ enrollment_type + iep_ifsp + subsidy + dll_status`

| Predictor | β | p-value | Interpretation |
|---|---|---|---|
| Full-time enrollment | +0.091 | < .001 | Strongest positive predictor — FT children attend significantly more |
| Subsidy recipient | +0.044 | .031 | Positive association — subsidy families are engaged, not disengaged |
| DLL status | −0.021 | .214 | Not significant at α = .05 |
| IEP / IFSP | −0.058 | .030 | Small negative effect — likely reflects coordination burden on families, not disengagement |

**Model fit:** R² = 0.38 · Adjusted R² = 0.36 · F(4,128) = 19.6, p < .001

The model explains 38% of the variation in attendance across children — a solid fit for real-world program data where unmeasured factors like family transportation, work schedules, and household instability account for a large share of absenteeism. The IEP finding is worth noting: children with IEPs attend slightly less, but the direction is better understood as a family coordination burden rather than a lack of engagement with the program.

### SPSS v28 — one-way ANOVA (CLASS Emotional Support by age group)

A one-way ANOVA was conducted in SPSS to test whether CLASS Emotional Support scores differed by age group (3–4 year classrooms vs. 4–5 year classrooms).

- F(1,5) = 4.82, p = .08
- η² = 0.18 (medium-large effect size)
- Cronbach's α = 0.86 (strong internal reliability of CLASS ES items)

The result trends toward significance but does not cross the α = .05 threshold with only 7 classrooms. The effect size (η² = 0.18) suggests the pattern is real — 4–5 year old classrooms may score modestly higher on Emotional Support — but a larger sample is needed to confirm it. This is an honest, appropriate interpretation: the data raises a hypothesis, not a conclusion.

### Power BI — dashboard design

The dashboard was designed to surface the story in the data immediately for a non-statistical audience — program directors, regional managers, and QI staff who need to make decisions, not run models. Key design choices:

- KPI cards up top for the numbers that matter at a glance
- ECERS-3 subscale bars with a "good" threshold line so the structural gap is visually obvious
- Scatter plot showing CLASS Emotional Support vs. ECERS composite — the point of that chart is that they *don't* correlate, which is the core analytical argument of the project
- ASQ-3 domains ranked high to low so the follow-up priority (personal-social) is immediately clear
- Plain-language annotations under the R and SPSS charts translating statistical output into program implications

### ECE assessment frameworks applied

- **ECERS-3** — Early Childhood Environment Rating Scale, 3rd edition. 7-point scale across 6 subscales. Structural subscale scores reflect facility constraints (building age, fixed layout, outdoor access) that are independent of teacher quality or program practices. This project demonstrates the importance of separating structural and process subscales when interpreting ECERS-3 data.
- **CLASS** — Classroom Assessment Scoring System. Emotional Support, Classroom Organization, and Instructional Support domains. Observation-based and immune to physical facility constraints — a key counterweight to ECERS-3 structural scores.
- **ASQ-3** — Ages & Stages Questionnaire, 3rd edition. Developmental screening across five domains. Child-level data in Sheet 2 reflects realistic distribution of above/below-cutoff scores, with IEP-flagged children showing lower scores in personal-social and problem-solving domains.
- **TS GOLD** — Teaching Strategies GOLD referenced in methodology as the ongoing observational assessment system operating alongside ASQ-3 in the program, reflecting the multi-instrument data environment common in large ECE organizations.

---

## Key findings

| Finding | Data point | Program implication |
|---|---|---|
| Structural subscale drags ECERS composite | Avg structural score: 2.4/7 | Building constraints, not teacher quality — annotate in all external reporting |
| CLASS Emotional Support is high program-wide | Avg ES: 6.1/7 | Strong teacher-child interactions persist despite low environment scores |
| Personal-social is lowest ASQ-3 domain | 58% of children on track | Targeted social-emotional support needed — review Pyramid Model tier placement |
| FT enrollment predicts attendance | β = +0.091, p < .001 | Conversion outreach to PT families is a high-leverage QI target |
| CR06 carries disproportionate load | 4 IEPs, 79.8% attendance, vacant co-teacher | Resource allocation issue — staffing gap, not teacher performance |
| Handwashing compliance inconsistent | Range: 58%–84% across classrooms | Facility and routine constraints explain low-scoring rooms — targeted coaching indicated |

---

## Equity flags

Two classrooms warrant priority attention on equity grounds:

**CR06** — This classroom has the highest IEP load in the cohort (4 children), the lowest attendance rate (79.8%), and has been operating without a co-teacher since September. The combination of high-needs enrollment and under-resourcing is a resource equity issue. Low scores in this classroom should be interpreted in that context, not as a reflection of teacher effectiveness.

**Handwashing compliance in CR03 and CR06** — Both rooms score lowest on handwashing compliance. Both also have structural and staffing constraints that reduce the time and space available for consistent personal care routines. Program-wide PD is not the right response. Targeted environmental and scheduling support is.

---

## Files in this repo

```
ece-classroom-quality/
├── data/
│   └── ECE_Program_Data_Synthetic.xlsx   ← two-sheet workbook (classroom + child records)
├── analysis/
│   ├── attendance_regression.R            ← R script, lm() output
│   └── class_anova_spss.spv              ← SPSS output file
├── dashboard/
│   └── index.html                         ← interactive Power BI-style dashboard
└── README.md
```

---

## Skills demonstrated

`R linear regression` · `SPSS one-way ANOVA` · `effect size interpretation` · `Power BI dashboard design` · `ECERS-3 subscale analysis` · `CLASS observation data` · `ASQ-3 developmental screening` · `TS GOLD` · `IEP/IFSP data management` · `DLL population analysis` · `equity-disaggregated reporting` · `missing data handling` · `ECE quality improvement` · `practitioner-informed data design` · `data storytelling for non-technical audiences`

---

*Practitioner-derived synthetic dataset. Built to reflect realistic ECE program conditions including missing values, outlier classrooms, and assessment data from a multi-instrument environment (ECERS-3, CLASS, ASQ-3). Designed to demonstrate analytical methodology relevant to large multi-site ECE organizations.*
