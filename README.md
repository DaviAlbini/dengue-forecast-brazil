# Dengue Outbreak Prediction in Brazil

Predictive modeling of dengue fever outbreaks across 13 municipalities in Southeast Brazil, combining epidemiological surveillance data with lagged climate variables.

Published as a graduate research paper — **MBA in Data Science & Analytics, USP/Esalq (2025)**.

📄 [Read the full paper (PDF)](./Article.pdf) · 📓 [View the notebook on nbviewer](https://nbviewer.org/github/DaviAlbini/dengue-forecast-brazil/blob/main/DengueAnalysis.ipynb)

---

## Overview

Dengue incidence in Brazil grew **442% between 2000 and 2024**, peaking at 6.4 million cases in 2024 — the largest outbreak ever recorded in the country. Reactive surveillance is insufficient. This study builds a **reproducible, municipality-level, weekly predictive pipeline** that anticipates epidemic waves using only publicly available data.

### Research Question
Can we predict dengue case counts at the municipal level — weekly and in advance — using historical notification series and lagged climate variables from public sources?

---

## Data Sources

| Source | Data | Period |
|---|---|---|
| DataSUS / SINAN (via Base dos Dados / BigQuery) | Weekly dengue notifications | 2000–2025 |
| INMET | Daily climate variables (8 features) | 2010–2025 |
| IBGE | Population estimates | 2013–2024 |

**Climate variables:** total precipitation, atmospheric pressure, mean/max/min temperature, mean/min relative humidity, wind gust speed.

---

## Methodology

**Study area:** 13 municipalities in Southeast Brazil selected by two criteria — population ≥ 500k and average dengue incidence ≥ 500 cases/100k inhabitants (2013–2024).

**Test window:** 2023 (stable epidemic year, avoiding the anomalous 2024 outbreak).

**Station matching:** Haversine metric + Ball Tree algorithm to assign the nearest INMET weather station to each municipality.

**Lag analysis:** Temporal displacement of climate variables tested to identify lagged correlations with notification series — capturing the biological cycle between rainfall events and case reporting.

**Stationarity tests:** ADF, KPSS, and Phillips-Perron applied to all series prior to modeling.

### Models Evaluated

| Family | Models |
|---|---|
| Simple baselines | Naïve, Mean, Drift |
| Exponential smoothing | SES, Holt, Holt-Winters (Additive) |
| Classical time series | ARIMA, SARIMA (52-week seasonality) |
| Count GLMs | Poisson regression, Negative Binomial |
| Tree-based ensembles | Decision Tree, Random Forest, XGBoost |
| Neural networks | LSTM (univariate / multivariate / climate-only) |

**Evaluation metrics:** RMSE (primary), MAE, MAPE. Best model selected per municipality.

---

## Key Findings

- **SARIMA** outperformed all models in municipalities with stable annual seasonality and strong autocorrelation — competing with or beating more complex approaches in 7 of 13 cities.
- **LSTM (history + climate)** achieved best temporal adherence in climate-sensitive municipalities (Niterói, São José dos Campos, among others).
- **XGBoost** was the best-performing tree model overall, capturing inflection points more accurately than Random Forest and Decision Tree.
- **GLM (Negative Binomial)** handled overdispersion well and outperformed univariate baselines in São Paulo.
- The optimal model **varied by municipality**, reflecting heterogeneous climate-dengue coupling across the region.
- Incorporating lagged climate variables improved first-semester predictions — exactly where univariate models failed most.

---

## Stack

`Python` · `pandas` · `NumPy` · `scikit-learn` · `XGBoost` · `statsmodels` · `TensorFlow/Keras (LSTM)` · `Google BigQuery` · `HydroBr` · `matplotlib` · `R (ggplot2)`

---

## Author

**Davi Albini** — Bioprocess & Biotechnology Engineer · MBA Data Science & Analytics (USP/Esalq)

- 🔗 [LinkedIn](https://www.linkedin.com/in/davialbini/)
- 📧 davialb23@gmail.com
