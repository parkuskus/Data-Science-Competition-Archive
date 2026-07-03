# Health Insurance Claim Trend Forecasting

> **Competition**: DSC MCF ITB
> **Source**: Kaggle
> **Metric**: MAPE (Mean Absolute Percentage Error) | **Team**: GakKuliah

---

## Problem Overview

Forecast monthly claim frequency, severity, and total claims for a health insurance portfolio (4,096 policies, 5,781 claims) over H2 2025 (Aug–Dec). The challenge involves 25.5% YoY claim growth and the need to predict three interdependent targets simultaneously.

---

## Approach: Multi-Model Ensemble with Disease-Specific Decomposition

### Data Preprocessing

| Step | Method |
|------|--------|
| Date parsing | `pd.to_datetime` with `errors='coerce'` |
| Paid claims filter | `Status Klaim == 'PAID'` |
| Missing values | Median imputation for external features |
| Location grouping | Indonesia / Singapore / Malaysia / Others |
| IP/OP mapping | `{'IP':'IP', 'OP':'OP', 'ODC':'OP', 'ODS':'OP'}` |
| Chronic identification | ICD N18/N19 + high-frequency policyholders (>20 claims) |

### Feature Engineering

| Feature Type | Method |
|--------------|--------|
| Monthly aggregation | Count, sum, mean per month (19 months: 2024-01 to 2025-07) |
| IP/OP severity | Separate IP and OP claim amounts |
| Location ratios | Singapore/Malaysia share per month |
| Disease classification | 5 major groups from ICD codes |
| External data | Working days, rainfall, holidays, FX rates |
| Cyclic encoding | `sin/cos` for month and quarter |

### Disease Classification (5 Major Groups)

| Group | ICD Range | Claims | Avg Severity |
|-------|-----------|--------|--------------|
| Cancer | C00-D49 | 914 (19.9%) | High (60-100M) |
| Circulatory | I00-I99 | 381 (8.3%) | High (40-60M) |
| Genitourinary | N00-N99 | 712 (15.5%) | Moderate |
| Eye/Ear | H00-H95 | 548 (11.9%) | Moderate |
| Respiratory | J00-J99 | 157 (3.4%) | Low-Moderate |

### Model Architecture

The pipeline uses a **10-phase architecture**:

| Phase | Description |
|-------|-------------|
| 1 | Data loading + disease classification + external data merge |
| 2 | Bayesian Poisson-Gamma / Exponential-InverseGamma baseline |
| 3 | Monthly time series + internal + external features |
| 3B | Forecast H2 2025 features using H2 2024 seasonal template |
| 4 | ARIMA / SARIMAX with exogenous variables (working days + rainfall) |
| 5 | LSTM with auxiliary features (3 bagged runs) |
| 6 | 19+ base strategies (Bayesian, trend, disease-specific) |
| 7 | Bagging (50x bootstrap with Linear/Ridge/Lasso) |
| 8 | Stacking (LOO meta-learner, 5 base models) |
| 9 | 30+ ensemble combinations |
| 10 | Ranking + 60B constraint enforcement + submission |

### Key Strategies

| Strategy | Description |
|----------|-------------|
| S1_Bayesian | Calibrated Poisson-Gamma baseline |
| S2_WDTrend | Working days + trend linear regression |
| S14_IPOPComp | IP/OP compositional severity |
| S17_CancerWeighted | Cancer-specific severity decomposition |
| S18_CirculatoryWeighted | Circulatory-specific severity |
| S19_DiseaseDecomposed | Full 5-disease severity decomposition |
| S9_SARIMAX_Real | SARIMAX with real external exogenous |
| S11_LSTM | LSTM with working days + rainfall aux |

### 60B Constraint

All ensembles are scaled to exactly **60B IDR H2 total** using sqrt scaling:

```python
scale = target / h2
t_new = total * scale
f_new = freq * np.sqrt(scale)
s_new = t_new / f_new
```

### Ensemble Examples

| Ensemble | Composition |
|----------|-------------|
| E1_Conservative | 50% V7 + 30% WDTrend + 20% RidgeReal |
| E21_CancerWeighted | 35% CancerSev + 30% V7 + 20% RidgeSev + 15% WDTrend |
| E30_V12_Flagship | Comprehensive 5-disease mix + all proven strategies |

---

## Results

| Version | Score (MAPE) | Target |
|---------|--------------|--------|
| V7 | 9.0% | Baseline |
| V8 | 8.5% | Beat V7 |
| V10 | 8.5% | Real external data |
| V11 | 5.06% | Disease-specific |
| V12 | **<5.0%** | 5-disease decomposition |

### V12 Key Improvements

- 5 major disease groups (Cancer, Circulatory, Genitourinary, Eye/Ear, Respiratory)
- Disease-specific severity decomposition from internal claims data
- 5 new disease-aware strategies (S17-S22)
- 30+ ensembles with disease awareness
- NO external cost proxies (100% internal data integrity)

---

## Directory Structure

```
Health-Insurance-Claim-Trend-Forecasting/
├── README.md
├── PROBLEM.md                         # Problem statement
├── GakKuliah.ipynb                    # End-to-end notebook (V10/V11/V12)
├── final_submission.csv               # Best submission
├── dataset/
│   ├── Data_Klaim.csv
│   ├── Data_Polis.csv
│   └── sample_submission.csv
└── dataset_extern/
    ├── external_data.csv
    ├── extended_external_data_real.csv
    ├── public-holiday2025.csv
    ├── public-holiday.csv
    ├── generate_cancer_cardio_cost_data.py
    ├── Consumer Price Index of Health Group and Sub (2022=100), 2024.csv
    ├── Consumer Price Index of Health Group and Sub (2022=100), 2025.csv
    ├── Data Historis SGD_IDR.csv
    └── Data Historis MYR_IDR.csv
```

---

## Prerequisites

```bash
pip install pandas numpy scipy scikit-learn statsmodels tensorflow matplotlib seaborn
```

---

## How to Run

1. Ensure `dataset/` folder contains `Data_Klaim.csv`, `Data_Polis.csv`, `sample_submission.csv`
2. Ensure `dataset_extern/` folder contains all external data files
3. Open `GakKuliah.ipynb`
4. Run the desired version cell (V10, V11, or V12)
5. Submission CSV files will be generated automatically

### What the Notebook Does

1. **Load Data** — Claims, policies, external data merge
2. **Disease Classification** — ICD code to 5 major groups mapping
3. **Bayesian Baseline** — Poisson-Gamma frequency, InvGamma severity
4. **Monthly Aggregation** — 19 months of features + H2 2024 template
5. **Time Series Models** — ARIMA, SARIMAX, Holt Damped
6. **Deep Learning** — LSTM with auxiliary features
7. **Strategy Generation** — 19+ base strategies
8. **Ensemble Building** — 30+ combinations with 60B constraint
9. **Ranking** — Internal quality metrics + submission generation

---

## Submission Format

```csv
id,value
2025_08_Claim_Frequency,234
2025_08_Claim_Severity,54000000
2025_08_Total_Claim,12700000000
...
```

- `id` — Format: `YYYY_MM_Metric_Name`
- `value` — Predicted value (frequency: count, severity: IDR amount, total: IDR amount)