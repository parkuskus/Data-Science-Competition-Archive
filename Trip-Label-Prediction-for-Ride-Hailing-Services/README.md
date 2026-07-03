# Trip Label Prediction for Ride-Hailing Services

> **Competition:** Data Science ARA 7.0 — UGM
> **Metric:** Macro F1-Score | **Team:** GakKuliah

---

## Problem Overview

In a Super App ride-hailing ecosystem, service quality and trip safety are critical for user satisfaction and operational sustainability. Risky driving behavior, inefficient routes, service complaints, and fraud indicators can degrade quality and increase operational risk if not detected early.

This project addresses the **UGM Data Science Competition (Anava UGM 2026)**, where the task is to classify each trip into one of five quality categories:

| Label | Description |
|-------|-------------|
| `Perfect_Trip` | No issues detected |
| `Safety_Violation` | Risky driving behavior |
| `Navigation_Issue` | Inefficient or incorrect routing |
| `Service_Complaint` | Customer service problems |
| `Fraud_Indication` | Suspicious or fraudulent activity |

The evaluation metric is **Macro F1-Score**, ensuring equal weighting across all classes despite severe imbalance (majority of trips are `Perfect_Trip`).

$$
\text{Macro F1} = \frac{1}{K} \sum_{k=1}^{K} F1_k, \quad F1_k = 2 \cdot \frac{P_k \cdot R_k}{P_k + R_k}
$$

where $P_k = \frac{TP_k}{TP_k + FP_k}$ and $R_k = \frac{TP_k}{TP_k + FN_k}$.

### Dataset

- **Train:** CSV with 24 features + target label
- **Test:** CSV with 24 features (no labels)
- **Key challenges:**
  - Severe class imbalance (`Perfect_Trip` dominates)
  - Missing values across multiple columns
  - Highly heterogeneous driver devices
  - Large data volume

### Features

| # | Feature | Type | Description |
|---|---------|------|-------------|
| 1 | `Trip_ID` | ID | Unique trip identifier |
| 2 | `Timestamp` | Temporal | Order time (ISO-8601) |
| 3 | `Pickup_Lat` | Geo | Pickup latitude |
| 4 | `Pickup_Long` | Geo | Pickup longitude |
| 5 | `Dropoff_Lat` | Geo | Dropoff latitude |
| 6 | `Dropoff_Long` | Geo | Dropoff longitude |
| 7 | `GPS_Accuracy_M` | Sensor | GPS accuracy (meters) |
| 8 | `Distance_KM` | Trip | Travel distance (km) |
| 9 | `Est_Price_IDR` | Transaction | Estimated price (IDR) |
| 10 | `Surge_Multiplier` | Transaction | Demand surge factor |
| 11 | `Accel_X` | Sensor | Accelerometer X-axis (m/s²) |
| 12 | `Accel_Y` | Sensor | Accelerometer Y-axis (m/s²) |
| 13 | `Accel_Z` | Sensor | Accelerometer Z-axis (m/s²) |
| 14 | `Gyro_Z` | Sensor | Gyroscope rotational data |
| 15 | `Pickup_Zone` | Categorical | Pickup administrative zone |
| 16 | `Dropoff_Zone` | Categorical | Dropoff administrative zone |
| 17 | `Device_FP` | Categorical | Driver device info |
| 18 | `Promo_Code` | Transaction | Promo voucher code |
| 19 | `Car_Model` | Categorical | Vehicle model |
| 20 | `Payment_Method` | Transaction | Payment method |
| 21 | `Weather` | Environmental | Weather condition |
| 22 | `Traffic` | Environmental | Traffic condition |
| 23 | `Battery_Level` | Device | Battery percentage (%) |
| 24 | `Signal_Strength` | Device | Cellular network type |
| 25 | `Trip_Label` | Target | Quality classification |

---

## Approach: Two-Stage LightGBM Cascade

The pipeline uses a **cascaded two-stage classification** approach rather than direct multiclass.

### Why Two Stages?

Stage 1 focuses on the easier binary task — *"is this trip problematic?"* — with tuned thresholding for high recall. Stage 2 then specializes in fine-grained problem classification among the non-perfect trips.

```
Input Data
    │
    ▼
[Feature Engineering]
    │
    ▼
[Stage 1: Binary LightGBM] ──► Perfect_Trip vs Problem
    │                                │
    │  (if Problem)                  │  (if Perfect)
    ▼                                ▼
[Stage 2: Multiclass LightGBM]  Default = Perfect_Trip
    │                                │
    ▼                                ▼
[Problem Type Label]            [Perfect_Trip]
    │                                │
    └────────── Merge All ◄──────────┘
                    │
                    ▼
            submission.csv
```

### Feature Engineering

| Feature | Purpose |
|---------|---------|
| `haversine_km` | Straight-line distance between pickup & dropoff |
| `dist_ratio` | Deviation between straight-line vs reported distance |
| `price_per_km` | Price efficiency per kilometer |
| `surge_flag` | High-demand indicator (binary) |
| `price_vs_zone` | Price relative to pickup zone average |
| `promo_flag` | Promo usage indicator (binary) |
| `Hour` | Time of day (0–23) |
| `DayOfWeek` | Day of week (0=Mon, 6=Sun) |
| `IsWeekend` | Weekend flag (binary) |
| `is_normal_dist` | Distance anomaly flag (Stage 2 only) |
| `is_normal_price` | Price anomaly flag (Stage 2 only) |

#### Formulas

$$
\text{haversine\_km} = 6371 \times 2 \arcsin\!\left(\sqrt{\sin^2\!\left(\frac{\Delta lat}{2}\right) + \cos(lat_1) \cdot \cos(lat_2) \cdot \sin^2\!\left(\frac{\Delta lon}{2}\right)}\right)
$$

$$
\text{dist\_ratio} = \frac{\text{haversine\_km}}{\text{Distance\_KM} + 10^{-3}}, \qquad \text{price\_per\_km} = \frac{\text{Est\_Price\_IDR}}{\text{Distance\_KM} + 10^{-3}}
$$

$$
\text{surge\_flag} = \mathbb{1}(\text{Surge\_Multiplier} > 1), \qquad \text{promo\_flag} = \mathbb{1}(\text{Promo\_Code} \neq \text{NaN})
$$

$$
\text{price\_vs\_zone} = \frac{\text{price\_per\_km}}{\overline{\text{price}}_{\text{zone}} + 10^{-3}}
$$

$$
\text{is\_normal\_dist} = \mathbb{1}\!\left(|\text{dist\_ratio} - 1| < 0.15\right), \qquad \text{is\_normal\_price} = \mathbb{1}\!\left(|\text{price\_vs\_zone} - 1| < 0.15\right)
$$

> $\mathbb{1}(\cdot)$ denotes the indicator function. $\overline{\text{price}}_{\text{zone}}$ is the mean `price_per_km` per `Pickup_Zone`.

**Categorical features** (native LightGBM handling): `Pickup_Zone`, `Dropoff_Zone`, `Device_FP`, `Car_Model`, `Payment_Method`, `Weather`, `Traffic`, `Signal_Strength`

**Imputation:** Median for numeric columns, `Surge_Multiplier` filled with 1.0, `Battery_Level` string-cleaned before conversion.

### Model Configuration

#### Stage 1 — Binary (Perfect vs Problem)

| Parameter | Variant 1 | Variant 2 |
|-----------|-----------|-----------|
| `objective` | `binary` | `binary` |
| `n_estimators` | 600 | 900 |
| `learning_rate` | 0.06 | 0.05 |
| `num_leaves` | 48 | 64 |
| `min_data_in_leaf` | 120 | 80 |
| `subsample` | 0.85 | 0.85 |
| `colsample_bytree` | 0.85 | 0.85 |
| `class_weight` | `{0:1.0, 1:1.7}` | `{0:1.0, 1:1.6}` |
| `early_stopping` | 60 rounds | 80 rounds |

**Threshold tuning:** Grid search with constraint $R \geq 0.72$, maximize precision.

- Variant 1: $\theta \in \text{linspace}(0.36, 0.42, 25)$
- Variant 2: $\theta \in \text{linspace}(0.10, 0.45, 70)$

#### Stage 2 — Multiclass (Problem Type Classification)

| Parameter | Value |
|-----------|-------|
| `objective` | `multiclass` |
| `n_estimators` | 1200 |
| `learning_rate` | 0.04 |
| `num_leaves` | 72 |
| `min_data_in_leaf` | 40 |
| `class_weight` | `balanced` |
| `early_stopping` | 100 rounds |

### Results

| Variant | Macro F1-Score |
|---------|---------------|
| **Variant 1** | **0.59376** |
| Variant 2 | 0.59363 |

---

## Directory Structure

```
Trip-Label-Prediction-for-Ride-Hailing-Services/
├── README.md
├── Notebook_DATAVERS0005008_GakKuliah.ipynb   # End-to-end notebook
└── markdown.txt                               # Variable descriptions (from committee)
```

---

## Prerequisites

```bash
pip install pandas numpy lightgbm scikit-learn joblib
```

### Dataset Setup

Download the competition dataset and place the following files in the project root:

```
train.csv              # Training data with Trip_Label (~2.5 GB)
test.csv               # Test data (~1.3 GB)
sample_submission.csv  # Submission template
```

---

## How to Run

### Option 1: Run the Notebook

```bash
jupyter notebook "Notebook_DATAVERS0005008_GakKuliah.ipynb"
```

Run all cells sequentially. The notebook handles:

1. **Data Loading** — Load train/test CSV, parse Timestamp
2. **Feature Engineering** — Haversine, price ratios, temporal features, anomaly flags
3. **Preprocessing** — Imputation, type casting, 85/15 stratified train/val split
4. **Stage 1 Training** — Binary LightGBM with threshold tuning
5. **Stage 2 Training** — Multiclass LightGBM on non-perfect trips only
6. **Inference** — Stage 1 → Stage 2 cascade → merge predictions
7. **Submission** — Save CSV with `Trip_ID, Trip_Label`

### Option 2: Use Pre-trained Models

If you have the saved `.joblib` artifacts, load them and apply the same feature engineering:

```python
import joblib
import pandas as pd

# Load artifacts
stage1_model = joblib.load("stage1_lgbm_model.joblib")
stage1_threshold = joblib.load("stage1_best_threshold.joblib")
stage2_model = joblib.load("stage2_lgbm_model.joblib")
label_encoder = joblib.load("stage2_label_encoder.joblib")

# Apply same feature engineering to test data, then:
# Stage 1: binary prediction
stage1_proba = stage1_model.predict_proba(X_test)[:, 1]
stage1_pred = (stage1_proba >= stage1_threshold).astype(int)

# Stage 2: multiclass prediction for flagged trips
problem_mask = stage1_pred == 1
if problem_mask.any():
    stage2_pred = label_encoder.inverse_transform(
        stage2_model.predict(X_test[problem_mask])
    )
    submission.loc[problem_mask, "Trip_Label"] = stage2_pred
```

---

## Submission Format

```csv
Trip_ID,Trip_Label
TRIP-19283746,Perfect_Trip
TRIP-91827364,Fraud_Indication
TRIP-11223344,Safety_Violation
```

- `Trip_ID` — unique trip identifier
- `Trip_Label` — predicted quality category
