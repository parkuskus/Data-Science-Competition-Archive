# LLM Sentiment Analysis

> **Competition**: DAC 2025 — Data Analysis Competition (ITS)
> **Metric**: F1 Score (Macro) | **Team**: epilogkuskus

---

## Problem Overview

Classify sentiment (Negative/Neutral-Mixed/Positive) of user reviews across 6 LLM platforms (GPT, Claude, Gemini, Grok, Perplexity, DeepSeek). Key challenges: severe class imbalance, bilingual text (English/Indonesian), and informal writing styles.

---

## Approach: XLM-Roberta with Advanced Loss Functions

### Data Preprocessing

| Step | Method |
|------|--------|
| Slang normalization | Custom `slang_dict` for Indonesian & English abbreviations |
| Emoji handling | `emoji` library for emoji processing |
| Text cleaning | URL removal, special character handling |
| Class imbalance | `compute_class_weight('balanced')` + minority class augmentation |
| Train/Val split | 85/15 stratified split |

### Data Augmentation

| Strategy | Description |
|----------|-------------|
| Minority oversampling | Augment minority classes to 50% of majority class |
| Class weighting | Inverse frequency weights: Neg=2.60, Neutral=6.36, Pos=0.41 |

### Model Configuration

| Parameter | Value |
|-----------|-------|
| Base model | `xlm-roberta-base` |
| Sentiment model | `cardiffnlp/twitter-xlm-roberta-base-sentiment` |
| Max sequence length | 256 tokens |
| Batch size | 8 (train) / 16 (eval) |
| Gradient accumulation | 2 steps |
| Epochs | 4 (early stopping patience=3) |
| Learning rate | 1e-5 |
| LR scheduler | Cosine with 10% warmup |
| Weight decay | 0.01 |
| FP16 | Enabled |
| Seed | 42 |

### Advanced Loss Functions

| Function | Description |
|----------|-------------|
| **RobustFocalLoss** | Focal loss with label smoothing, handles class imbalance |
| **CombinedLoss** | 70% FocalLoss + 30% CrossEntropy, balanced approach |
| Label smoothing | 0.1 (reduces overconfidence) |
| Focal gamma | 2.0 (down-weights easy examples) |

```python
class CombinedLoss(nn.Module):
    def __init__(self, focal_weight=0.7, ce_weight=0.3, **kwargs):
        self.focal_loss = RobustFocalLoss(**kwargs)
        self.ce_weight = ce_weight
        self.focal_weight = focal_weight

    def forward(self, inputs, targets):
        focal = self.focal_loss(inputs, targets)
        ce = F.cross_entropy(inputs, targets, weight=self.alpha)
        return self.focal_weight * focal + self.ce_weight * ce
```

### Training Pipeline

| Component | Choice |
|-----------|--------|
| Trainer | Custom `AdvancedTrainer` with advanced loss |
| Metric | F1 macro (primary), F1 weighted |
| Early stopping | Patience=3 epochs |
| Cross-validation | 3-fold stratified (ensemble mode) |
| Ensemble | Average predictions across folds + models |

---

## Results

| Model | Val F1 (Macro) |
|-------|----------------|
| xlm-roberta-base (single) | ~0.65-0.70 |
| Ensemble (3-fold × 2 models) | ~0.68-0.73 |

### Prediction Distribution (Test Set)

| Class | Count |
|-------|-------|
| Negative (0) | ~15-20% |
| Neutral/Mixed (1) | ~5-10% |
| Positive (2) | ~70-80% |

---

## Directory Structure

```
LLM-Sentiment-Analysis/
├── README.md
├── PROBLEM.md                              # Problem statement
├── Source Code_epilogkuskus.ipynb          # End-to-end notebook
├── Answer Sheets_epilogkuskus.pdf          # Written report
├── CSV_epilogkuskus.csv                    # Submission CSV
└── dataset/
    ├── Train/
    │   ├── gpt.csv
    │   ├── claude.csv
    │   ├── gemini.csv
    │   ├── grok.csv
    │   ├── perplexity.csv
    │   └── deepseek.csv
    ├── Test/
    │   ├── gpt.csv
    │   ├── claude.csv
    │   ├── gemini.csv
    │   ├── grok.csv
    │   ├── perplexity.csv
    │   └── deepseek.csv
    └── sample_submission.csv
```

---

## Prerequisites

```bash
pip install torch transformers datasets evaluate scikit-learn emoji pandas numpy matplotlib seaborn
```

---

## How to Run

1. Ensure `dataset/` folder contains `Train/` and `Test/` subfolders with all CSV files
2. Open `Source Code_epilogkuskus.ipynb`
3. Run all cells
4. Submission CSV will be saved to `/kaggle/working/results/submission.csv`

### What the Notebook Does

1. **Data Loading** — Load train/test CSVs from 6 LLM platforms
2. **Preprocessing** — Slang normalization, emoji handling, text cleaning
3. **Augmentation** — Minority class oversampling for balance
4. **Model Setup** — XLM-Roberta with custom config (dropout, attention dropout)
5. **Training** — AdvancedTrainer with CombinedLoss (Focal + CE)
6. **Prediction** — Argmax on logits for final sentiment labels
7. **Evaluation** — Confusion matrix, prediction distribution visualization

---

## Submission Format

```csv
CommentId,Sentiment
gpt_1,2
gpt_2,0
gpt_3,1
...
```

- `CommentId` — Unique test identifier (format: `{platform}_{number}`)
- `Sentiment` — Predicted class (0=Negative, 1=Neutral/Mixed, 2=Positive)