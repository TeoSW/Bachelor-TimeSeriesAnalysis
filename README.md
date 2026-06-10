# Forecasting Project — From Potential to Performance: Using AI Algorithms in Urban Development

**Course:** Forecasting (Previziune Economică)
**Team:** Constantin Teodor-Vasile, Erhan Teodora-Miruna, Durdureanu Adina
**Program:** Applied Statistics and Data Science (ASDS) — ASE Bucharest, FCSIE

**Files:** `projPreviziune.docx` · `prezentarePreviziune.pptx` · `previziuneBD.xlsx`

---

## Overview

This project uses predictive modeling to analyze Bucharest's position in the **Smart City Index 2024** (published by IMD and SUTD) and to identify the factors that most influence a city's ranking. Beyond prediction, the project simulates concrete infrastructure improvement scenarios to estimate how Bucharest could climb the global Smart City ranking.

## Research Objectives

1. **Build a predictive model** of a city's Smart City ranking using statistical and socio-economic indicators (HDI, infrastructure score, technology score).
2. **Identify the most relevant factors** driving Smart City performance through automated variable selection.
3. **Simulate ranking improvements** for Bucharest by testing hypothetical upgrades to infrastructure and technology scores.

## Hypotheses

- A city's position in the Smart City Index can be accurately estimated using a predictive model built on quantitative and qualitative indicators.
- Infrastructure and technology are the dominant predictors of Smart City ranking, overriding general human development metrics.
- Simulating targeted improvements to these factors can forecast a significant ranking uplift for Bucharest.

---

## Dataset

**Source:** IMD Smart City Index 2024 Full Report — [coit.es link](https://www.coit.es/sites/default/files/imd_-smartcityindex-2024-full-report.pdf)

**Statistical unit:** Individual cities (one observation per city)  
**Reference period:** 2024  
**Sample size:** ~140 cities worldwide

| Variable | Type | Description |
|---|---|---|
| `Country` | Categorical | Country of origin |
| `CountryHDI` | Quantitative | Human Development Index at country level |
| `City` | Identifier | City name |
| `HDIcity` | Quantitative | Estimated HDI at city level |
| `SMCR_2024` | Quantitative | Final Smart City ranking (2024) — **target variable** |
| `SmartCityRating2024` | Ordered factor | Rating class: AAA → D |
| `Structure2024` | Ordered factor | Urban infrastructure score: AAA → D |
| `Technology2024` | Ordered factor | Urban technology/digitalization score: AAA → D |

Rating scale (ordered): `AAA > AA > A > BBB > BB > B > CCC > CC > C > D`

---

## Methodology

### Model 1 — Random Forest (Classification)

- **Goal:** Classify cities into Smart City rating classes and predict Bucharest's class.
- **Implementation:** 700 trees; qualitative variables encoded as ordered factors.
- **Results:**
  - Overall accuracy: **83.10%** (OOB error: 16.90%)
  - Error stabilizes after ~300–400 trees.
  - Most important variables by Gini impurity reduction: **Structure2024 > Technology2024 > HDIcity > CountryHDI**
  - Bucharest predicted class: **B** → estimated rank: **77.8** (actual rank: 100)

### Model 2 — Stepwise Regression (Variable Selection)

- **Goal:** Identify the minimal set of variables that best predict the numerical ranking (SMCR_2024).
- **Method:** `stepAIC` with both forward and backward selection (R, `MASS` package).
- **Final model predictors:** `Structure2024` + `Technology2024` (HDI variables dropped as non-significant in presence of functional scores)
- **Model fit:**
  - R² = **0.8668**
  - Adjusted R² = **0.8473**
  - p-value < 2.2e-16

### Model 3 — Random Forest v2.0 (Scenario Simulation)

- **Goal:** Simulate what Bucharest's ranking would be under improved infrastructure/technology scores.
- **Scenarios tested:**

| Structure2024 | Technology2024 | Expected outcome |
|---|---|---|
| BB | B | Moderate improvement |
| BB | BB | Noticeable improvement |
| BBB | B | Significant improvement |
| BBB | BB | Best-case scenario |

---

## Key Findings

- **Infrastructure (Structure2024) is by far the most influential predictor** of Smart City ranking.
- Bucharest's HDI (0.926) is identical to Warsaw's (ranked 38th), yet Bucharest sits at rank 100 — revealing a structural performance gap, not a development gap.
- The gap between Bucharest's potential (estimated rank ~78) and actual rank (100) reflects deficiencies in urban mobility, digitalized public services, and coherent infrastructure — not a lack of resources.
- Cities in class A (London, Prague, Stockholm, Amsterdam) are characterized by stable governance, high digitalization, and integrated urban planning. Bucharest shares class B/BB with Milan, Zagreb, Madrid, and Dublin — cities in transition.

## Policy Recommendations (Objective 3)

To move from class B toward BB or BBB, Bucharest should:

- Adopt the **"15-minute city" model** — ensure essential services (education, health, commerce, transport) are accessible within 15 minutes.
- Expand **sustainable public transport** — electric buses, modern trams, dedicated lanes.
- Build a **coherent cycling network** — safe, connected, integrated at the metropolitan level.
- Reduce individual car traffic — restricted access zones, smart tolling, incentives for shared/electric mobility.

---

## Code

All analysis was conducted in **R**. The project includes three scripts:

| Script | Method | Key packages |
|---|---|---|
| `RF` | Random Forest (classification) | `randomForest`, `readxl` |
| `SW` | Stepwise Regression | `MASS`, `readxl`, `writexl` |
| `RF v2.0` | Random Forest (regression, scenario simulation) | `randomForest`, `readxl` |

---

## References

- Cugurullo, F. et al. (2024). *The rise of AI urbanism in post-smart cities.* Urban Studies, 61, 1168–1182. https://doi.org/10.1177/00420980231203386
- Agostinelli, S. et al. (2021). *Cyber-Physical Systems Improving Building Energy Management.* Energies, 14, 2338. https://doi.org/10.3390/en14082338
- Alahi, M.E.E. et al. (2023). *Integration of IoT-Enabled Technologies and AI for Smart City Scenario.* Sensors, 23, 5206. https://doi.org/10.3390/s23115206
- IMD & SUTD. (2024). *Smart City Index 2024 Full Report.* https://www.coit.es/sites/default/files/imd_-smartcityindex-2024-full-report.pdf
