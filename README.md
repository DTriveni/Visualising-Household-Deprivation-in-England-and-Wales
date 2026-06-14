# Visualising Household Deprivation in England and Wales
### A Comparative Analysis of the 2011 and 2021 Censuses

## Overview

This project analyses household deprivation across 175 local authorities in England and Wales using data from the 2011 and 2021 UK Censuses. It combines six deprivation-related variables — poverty dimensions, general health, economic activity, educational qualifications, central heating, and number of bedrooms — to examine spatial patterns, structural causes, and temporal change over a decade.

Four interactive Tableau dashboards are produced, supported by a Python data preparation and modelling pipeline.

---

## Research Questions

1. How does household deprivation map across England and Wales, and how has this changed from 2011 to 2021?
2. What hidden patterns lie within deprivation — do health, employment, education, and housing cluster together or separate?
3. Can 2011 socio-economic conditions predict 2021 poverty levels, and which local authorities deviate from expectations?

---

## Data Sources

> **Data Notice:** All data used in this project is sourced from the **UK Census (Office for National Statistics)** and is used strictly for **educational and study purposes** as part of the MSc Data Science and AI programme at the University of Bristol. The data is publicly available and no personal or identifiable information is included.

| Dataset | Source | Years |
|---|---|---|
| Households by deprivation dimensions | ONS Census | 2011, 2021 |
| General health (self-reported) | ONS Census | 2011, 2021 |
| Economic activity (employment status) | ONS Census | 2011, 2021 |
| Highest level of qualification | ONS Census | 2011, 2021 |
| Central heating type | ONS Census | 2011, 2021 |
| Number of bedrooms | ONS Census | 2011, 2021 |
| Census boundary changes | [ONS Census Maps](https://www.ons.gov.uk/releases/censusmapsupdatechangeovertime) | 2023 |

**Geographic scope:** County and unitary authority level — 175 matched areas across England and Wales.

---

## Methodology

### Data Preparation
- Removed regional aggregate rows; retained 175 individual local authorities
- Standardised category labels across 2011 and 2021 formats (health, heating, bedroom, economic activity variables)
- Resolved three boundary changes (Bournemouth/Poole merger; Cumbria and Northamptonshire splits) using population-based apportionment
- Converted all counts to percentages for cross-area comparability
- Derived overall deprivation rate: `Deprivation Rate = 100 − Not Deprived %`

### Dimensionality Reduction
- **PCA** on five selected features (after removing highly correlated pairs, r > 0.7); two components retained via Kaiser rule, explaining **74.6% of variance**
- **t-SNE** (perplexity = 40, run on top 3 PCA components) to identify discrete local clusters

### Predictive Modelling
- **Bayesian Linear Regression** with conjugate Normal-Inverse-Gamma priors; closed-form posterior (no MCMC required)
- Predicts 2021 deprivation from 2011 features; R² = 0.928, MAE = 1.011
- 95% credible intervals used to flag outlier local authorities

---

## Key Findings

- Overall deprivation fell from **58.7% to 52.4%** between 2011 and 2021; all 175 local authorities improved
- Inner London boroughs (e.g. Newham: −14.3pp, City of London: −14.6pp) improved the most — partly reflecting gentrification
- PCA reveals a **two-axis deprivation structure**: general deprivation (health + employment + education) and a separate rural fuel poverty axis (no central heating)
- Education is the most resistant dimension to change; qualification levels are a generational-scale variable
- The unemployment–health correlation collapsed from r = 0.554 (2011) to r = −0.038 (2021), likely reflecting COVID-19 furlough effects
- **Nine local authorities** deviate significantly from model predictions; Brighton and Hove underperformed (+3.3pp); City of London overperformed (−5.6pp)

---

## Dashboards

| Dashboard | Description |
|---|---|
| Dashboard 1 — Deprivation Overview | Choropleth map + deprivation dimension bar chart, filterable by year |
| Dashboard 2 — Change Over Time | Slope chart and change map showing 2011→2021 trajectories |
| Dashboard 3 — Dimensionality Reduction | PCA scatter with loadings + t-SNE cluster plot |
| Dashboard 4 — Predictive Model | Actual vs. predicted scatter, credible interval bands, and residual ranking |

---

## Tools and Libraries

| Tool | Purpose |
|---|---|
| Python 3.x | Data cleaning, feature engineering, PCA, t-SNE, Bayesian regression |
| pandas, numpy | Data manipulation |
| scikit-learn | PCA, t-SNE, model evaluation |
| scipy | Bayesian regression (conjugate priors) |
| Tableau | Interactive dashboard development |

---

## References

- Munzner, T. (2014). *Visualization Analysis and Design.* CRC Press.
- Cleveland, W.S. & McGill, R. (1984). Graphical perception. *Journal of the American Statistical Association*, 79(387), 531–554.
- Shneiderman, B. (1996). The eyes have it. *IEEE Symposium on Visual Languages*, 336–343.
- Townsend, P. (1987). Deprivation. *Journal of Social Policy*, 16(2), 125–146.
- Dorling et al. (2007). *Poverty, Wealth and Place in Britain, 1968 to 2005.* Policy Press.
- van der Maaten, L. & Hinton, G. (2008). Visualizing data using t-SNE. *JMLR*, 9, 2579–2605.
- MHCLG (2019). *The English Indices of Deprivation 2019.*

---

## Licence

This repository is submitted as coursework for the University of Bristol MSc Data Science and AI programme. The code and analysis are for academic use only. Census data is © Crown Copyright and used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
