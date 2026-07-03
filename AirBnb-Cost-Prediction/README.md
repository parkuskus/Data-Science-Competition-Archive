# Airbnb Cost Prediction

> **Competition**: SPARTA 2024 Data Science Competition
> **Source**: BukitVista (Kaggle)
> **Metric**: RMSE

---

## Problem Overview

Kompetisi ini menantang peserta untuk memprediksi harga sewa per malam dari listing Airbnb yang tersebar di berbagai kota dunia. Dataset mencakup 300,000 listing training dan 100,000 listing test dengan fitur seperti tipe properti, lokasi, atribut host, serta data ulasan dan ketersediaan.

---

## Approach: Ensemble & Gradient Boosting

### Data Preprocessing

| Step | Method |
|------|--------|
| Missing Values (Numeric) | `SimpleImputer(strategy='mean')` |
| Missing Values (Categorical) | `SimpleImputer(strategy='constant', fill_value='missing')` |
| Encoding (Tree-based) | `OrdinalEncoder` |
| Encoding (Neural Network) | `OneHotEncoder` |
| Scaling (Neural Network) | `StandardScaler` |
| Feature Selection | Drop kolom dengan >50% missing, drop kolom ID/redundan |

### Feature Selection

Kolom yang di-drop:
- `id`, `host_id` — identifier unik, tidak informatif
- `host_listings_count`, `number_of_reviews`, `number_of_reviews_l30d` — redundan
- `availability_30`, `availability_60`, `availability_90` — highly correlated dengan `availability_365`

Fitur numerik yang dipertahankan (17 kolom):
`host_total_listings_count`, `latitude`, `longitude`, `accommodates`, `bathrooms`, `bedrooms`, `beds`, `availability_365`, `number_of_reviews_ltm`, `review_scores_rating`, `review_scores_accuracy`, `review_scores_cleanliness`, `review_scores_checkin`, `review_scores_communication`, `review_scores_location`, `review_scores_value`, `reviews_per_month`

Kategorikal dengan cardinality < 10 di-encode ordinal.

### Models

| Model | Config | Val RMSE |
|-------|--------|----------|
| **XGBoost** | n_est=200, lr=0.1 | 110.92 |
| **CatBoost** | iter=1300, lr=0.1, depth=10 | **108.86** |
| **LightGBM** | n_est=2000, lr=0.1, leaves=70 | **108.57** |
| **Random Forest** | n_est=1200, depth=50 | 110.47 |
| **Stacking** | XGB + CatBoost + LGB → LGB meta | — |
| **Deep Learning** | 128→64, ReLU, Adam, EarlyStopping | — |

### Training Configuration

| Parameter | Value |
|-----------|-------|
| Split | 80/20 stratified |
| Seed | 42 |
| Metric | RMSE |

---

## Results Summary

**Best Model**: LightGBM (Val RMSE: **108.57**)

Performa cukup konsisten antara gradient boosting models (XGBoost, CatBoost, LightGBM, RF) dengan RMSE berkisar 108-111. Stacking dan deep learning belum memberikan improvement signifikan.

---

## Directory Structure

```
AirBnb-Cost-Prediction/
├── README.md
├── Problem.md                    # Problem statement
├── SPARTA-2024-DatSci-Compe.ipynb # End-to-end notebook
└── sample_submission.csv
```

---

## How to Run

```bash
pip install pandas numpy scikit-learn xgboost catboost lightgbm tensorflow scikeras
```

1. Buka `SPARTA-2024-DatSci-Compe.ipynb`
2. Pastikan `train.csv` dan `test.csv` ada di working directory
3. Run all cells

---

## Submission Format

```csv
id,price
123456,150.00
789012,200.00
...
```