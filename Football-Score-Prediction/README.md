# Football Score Prediction

> **Competition**: Yooji & Mina's World Cup Score Prediction
> **Metric**: Augmented Weighted MAE (AW-MAE)
> **Target**: `team_goals` dan `opp_goals` (integer)

---

## Problem Overview

Kompetisi ini menantang peserta untuk memprediksi skor pertandingan sepak bola dari seluruh dunia. Setiap pertandingan memiliki dua baris — satu untuk tim utama dan satu untuk lawan. Model harus memprediksi jumlah gol masing-masing.

**Tantangan utama**: Test set tidak memiliki fitur historis (ELO, rank, form) — semua harus direkonstruksi dari training data menggunakan chronological feature engineering.

---

## Approach: LightGBM + Chronological Feature Engineering

### Feature Engineering

Karena test set tidak memiliki fitur historis, pipeline membangun fitur secara kronologis dari training data:

| Feature Type | Method |
|--------------|--------|
| Expanding averages | `expanding().mean().shift(1)` per tim |
| Rolling windows | `rolling(5/10).mean().shift(1)` per tim |
| Home/Away form | Rolling stats terpisah untuk home/away |
| Head-to-Head | Expanding/rolling stats per pasangan tim |
| ELO momentum | `diff(5/10)` pada ELO rating |
| Days since last match | `diff().dt.days` per tim |

**Total fitur**: 89 (termasuk base features + chronological engineering)

### Preprocessing

| Step | Method |
|------|--------|
| Missing values | Median imputation dari train set |
| Categorical | LabelEncoder (fit on train only) |
| Target encoding | Tournament weights dari spesifikasi |

### Model Configuration

| Parameter | Value |
|-----------|-------|
| Model | LightGBM (Poisson objective) |
| n_estimators | 3000 (early stopping) |
| learning_rate | 0.02 |
| num_leaves | 127 |
| subsample | 0.8 |
| colsample_bytree | 0.7 |
| Validation | 5-fold TimeSeriesSplit |
| Seed | 42 |

### Training Strategy

- **Objective**: Poisson (cocok untuk count data seperti gol)
- **Validation**: TimeSeriesSplit (menghormati urutan waktu)
- **Early stopping**: 100 rounds
- **Two separate models**: Satu untuk `team_goals`, satu untuk `opp_goals`

---

## Results

| Metric | Value |
|--------|-------|
| OOF MAE team_goals | 0.9437 |
| OOF MAE opp_goals | 0.9180 |
| OOF Outcome Accuracy | 56.1% |
| OOF Exact Score Accuracy | 12.8% |
| Estimated AW-MAE | 2.7548 |

### Key Insights

- Outcome accuracy 56.1% — model cukup baik menebak menang/seri/kalah
- Exact score accuracy hanya 12.8% — prediksi skor tepat sulit dicapai
- Fitur chronologis (expanding/rolling averages) menjadi fitur paling penting

---

## Directory Structure

```
Football-Score-Prediction/
├── README.md
├── PROBLEM.md                    # Problem statement
├── football_score_prediction.ipynb # End-to-end notebook
├── dataset/
│   ├── train.csv
│   ├── test.csv
│   └── sample_submission.csv
└── outputs/
    ├── submission.csv
    ├── eda_*.png                 # EDA visualizations
    ├── feature_importance.png
    ├── oof_analysis.png
    └── test_predictions.png
```

---

## Prerequisites

```bash
pip install pandas numpy scikit-learn lightgbm optuna matplotlib seaborn
```

---

## How to Run

1. Buka `football_score_prediction.ipynb`
2. Pastikan `dataset/` folder berisi `train.csv`, `test.csv`, `sample_submission.csv`
3. Run all cells

### What the Notebook Does

1. **Load Data** — Baca train/test, deteksi fitur yang hilang di test
2. **EDA** — Distribusi target, korelasi, tren waktu, analisis turnamen
3. **Feature Engineering** — Chronological features tanpa data leakage
4. **Modeling** — LightGBM dengan Poisson objective + TimeSeriesSplit CV
5. **Validation** — OOF MAE, outcome accuracy, AW-MAE estimation
6. **Inference** — Prediksi test set + generate submission

---

## Submission Format

```csv
Id,team_goals,opp_goals
M034984_Seychelles,1,1
M034984_Mauritius,1,1
```