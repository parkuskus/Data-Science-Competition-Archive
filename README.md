# Data Science Competition Archive

A collection of data science competition projects from various university-level competitions across Indonesia and Southeast Asia. Each project contains problem statements, approach documentation, and end-to-end notebooks.

## Projects

| # | Project | Competition | Problem Type | Approach | Metric | Team | Year |
|---|---------|-------------|--------------|----------|--------|------|------|
| 1 | [Traditional House Classification](Traditional-House-Classification/) | Logika UI 2025 (Kaggle) | Image Classification (5 classes) | ResNet50 fine-tune + TTA | Macro F1-Score | GakKuliah | 2025 |
| 2 | [AirBnb Cost Prediction](AirBnb-Cost-Prediction/) | SPARTA 2024 (Kaggle) | Regression (Price) | XGBoost / CatBoost / LightGBM / RF / DL Ensemble | RMSE | GakKuliah | 2024 |
| 3 | [Football Score Prediction](Football-Score-Prediction/) | Yooji & Mina's World Cup | Time Series Regression (Goals) | LightGBM Poisson + Chronological FE | AW-MAE | Psi -1 | 2026 |
| 4 | [Health Insurance Claim Trend Forecasting](Health-Insurance-Claim-Trend-Forecasting/) | DSC MCF ITB (Kaggle) | Time Series Forecasting (Freq/Sev/Total) | Bayesian + ARIMA + LSTM + Disease-Specific Ensemble | MAPE | GakKuliah | 2026 |
| 5 | [LLM Sentiment Analysis](LLM-Sentiment-Analysis/) | DAC 2025 — ITS | NLP Classification (3 classes) | XLM-Roberta + Focal Loss + CombinedLoss | F1-Score (Macro) | epilogkuskus | 2025 |
| 6 | [Pothole Segmentation](Pothole-Segmentation/) | Data Science ARA 7.0 (Kaggle) | Image Segmentation (Binary) | MAnet (mit_b3) + Multi-scale TTA + Morphological Post-processing | Dice Coefficient | GakKuliah | — |
| 7 | [Face Anti-Spoofing Detection](Face-Spoofing-Detection/) | FindIT! 2026 — UGM | Image Classification (6 classes) | DINOv3 ViT-Large + Multi-Sample Dropout + Two-phase Training | Macro F1-Score | psi-1 | 2026 |
| 8 | [Trip Label Prediction](Trip-Label-Prediction-for-Ride-Hailing-Services/) | Anava UGM 2026 | Tabular Classification (5 classes) | Two-stage LightGBM Cascade + Threshold Tuning | Macro F1-Score | GakKuliah | 2026 |

## Problem Types

| Type | Projects |
|------|----------|
| Image Classification | Traditional House Classification, Face Anti-Spoofing Detection |
| Image Segmentation | Pothole Segmentation |
| Tabular Classification | Trip Label Prediction, LLM Sentiment Analysis |
| Regression | AirBnb Cost Prediction |
| Time Series Forecasting | Health Insurance Claim Trend Forecasting, Football Score Prediction |

## Tech Stack

| Category | Technologies |
|----------|-------------|
| Deep Learning | PyTorch, TensorFlow/Keras, timm, segmentation-models-pytorch |
| Gradient Boosting | XGBoost, CatBoost, LightGBM |
| NLP | HuggingFace Transformers (XLM-Roberta), datasets |
| Classical ML | Scikit-learn (Ridge, Lasso, Linear Regression) |
| Time Series | Statsmodels (ARIMA, SARIMAX, Exponential Smoothing) |
| Bayesian | Scipy (Poisson-Gamma, Exponential-InverseGamma) |
| Augmentation | Albumentations, torchvision transforms |
| Visualization | Matplotlib, Seaborn |

## Repository Structure

```
Data-Science-Competition-Archive/
├── README.md
├── STYLE.md                              # README style guide
├── Traditional-House-Classification/     # Image classification (5 Indonesian house types)
├── AirBnb-Cost-Prediction/              # Regression (listing price prediction)
├── Football-Score-Prediction/           # Time series (match score prediction)
├── Health-Insurance-Claim-Trend-Forecasting/  # Ensemble forecasting (claim trends)
├── LLM-Sentiment-Analysis/              # NLP (multilingual sentiment)
├── Pothole-Segmentation/                # Segmentation (road pothole detection)
├── Face-Spoofing-Detection/             # Classification (face anti-spoofing)
└── Trip-Label-Prediction-for-Ride-Hailing-Services/  # Tabular (trip quality)
```

## Quick Start

Each project folder contains:
- `README.md` — Problem overview, approach, results, directory structure
- `PROBLEM.md` — Full problem statement (if available)
- `*.ipynb` — End-to-end notebook (EDA → Training → Submission)
- `dataset/` — Data files (or instructions to download)

## License

This repository is for educational and archival purposes. Competition datasets are subject to their respective competition rules.