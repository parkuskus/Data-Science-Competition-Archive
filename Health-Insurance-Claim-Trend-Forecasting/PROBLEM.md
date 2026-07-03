# Health Insurance Claim Trend Forecasting

## Overview

Individual health insurance is a tool for mitigating personal financial risk from unexpected events such as illness or accidents, which can potentially cause significant costs and impact personal financial planning.

In recent years, one of the key issues has been the increase in individual health insurance claims, with a **25.5% increase** between January–June 2025 compared to the same period in 2024. This increase impacts premium adjustments, potentially making individual health insurance products less affordable. Therefore, data-driven analysis is needed to predict the factors most influential to claim values, enabling initiatives in risk selection, prevention, and early detection to minimize the impact of rising claims and maintain affordable premiums.

## Problem Statement

Participants are challenged to build a predictive model that can forecast three key metrics for **August to December 2025**:

1. **Claim Frequency** — Number of claims per month
2. **Claim Severity** — Average claim amount per month
3. **Total Claim** — Frequency × Severity per month

## Timeline

| Milestone | Date |
|-----------|------|
| Competition Start | Feb 14, 2026 |
| Competition End | Mar 15, 2026 |

## Dataset Description

The dataset consists of **4,096 insurance policy data** and **5,781 health insurance claims** for the period January 1, 2024 to July 31, 2025.

### Files

| File | Description |
|------|-------------|
| `Data_Klaim.csv` | Health insurance claim transactions up to 2025-07-31 |
| `Data_Polis.csv` | Master data containing all active policies |
| `sample_submission.csv` | Example submission format |

### Data_Klaim.csv — Claim Transactions

| Column | Description |
|--------|-------------|
| Claim ID | Unique identifier for each health insurance claim |
| Nomor Polis | Unique policy number of the insured |
| Reimburse/Cashless | Claim settlement method (R = Reimburse, C = Cashless) |
| Inpatient/Outpatient | Care category (IP = Inpatient, OP = Outpatient) |
| ICD Diagnosis | ICD classification code (e.g., `H26.9`) |
| ICD Description | Medical description based on ICD code |
| Status Klaim | Claim processing status (`Paid` or `Pending`) |
| Tanggal Pembayaran Klaim | Date when funds were disbursed |
| Tanggal Pasien Masuk RS | Hospital admission date |
| Tanggal Pasien Keluar RS | Hospital discharge date |
| Nominal Klaim Yang Disetujui | Approved claim amount |
| Nominal Biaya RS Yang Terjadi | Total hospital bill submitted |
| Lokasi RS | Hospital location |

### Data_Polis.csv — Policy Data

| Column | Description |
|--------|-------------|
| Nomor Polis | Unique policy number |
| Plan Code | Product code determining coverage region |
| Gender | Policyholder gender |
| Tanggal Lahir | Policyholder birth date |
| Tanggal Efektif Polis | Policy effective date |
| Domisili | Policyholder domicile |

#### Plan Code Coverage

| Code | Coverage |
|------|----------|
| M-001 | Worldwide |
| M-002 | Regional Asia |
| M-003 | Domestic Indonesia |

## Evaluation

### Metric: Mean Absolute Percentage Error (MAPE)

MAPE is chosen because it focuses on the percentage error relative to actual values, not absolute error. This is highly suitable for insurance business needs that care about prediction accuracy proportions, not just nominal differences.

### Scoring Calculation

1. Calculate MAPE for **Frequency** predictions
2. Calculate MAPE for **Severity** predictions
3. Calculate MAPE for **Total** predictions
4. **Final Score** = Average of the three MAPE values

By calculating MAPE separately for frequency, severity, and total claims, this scoring system encourages participants to build models that are accurate across all aspects, not just focused on one component.

## Leaderboard

Ranking is based on MAPE, where **lower scores achieve higher rankings**.

| Leaderboard | Description |
|-------------|-------------|
| Public | Based on 40% of test data (visible during competition) |
| Private | Based on 60% of test data (final ranking) |

**Final Score**: 100% Private Leaderboard

## Submission Format

```csv
id,value
2025_08_Claim_Frequency,0
2025_08_Claim_Severity,0
2025_08_Total_Claim,0
2025_09_Claim_Frequency,0
...
```

Each row contains:
- `id` — Metric identifier in format `YYYY_MM_Metric_Name`
- `value` — Predicted value