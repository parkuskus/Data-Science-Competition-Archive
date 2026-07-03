# Traditional House Classification

> **Competition:** Logika UI 2025 — Kaggle
> **Metric:** Macro F1-Score | **Team:** GakKuliah

---

## Problem Overview

As knowledge of traditional houses among younger generations declines, cultural preservation efforts become increasingly important. This competition challenges participants to classify images of Indonesian traditional houses into **5 architectural styles** from across the archipelago.

The dataset contains thousands of images with variations in lighting, viewing angles, and image quality — reflecting real-world conditions that demand robust generalization.

### Classes

| Label | Description |
|-------|-------------|
| `balinese` | Traditional Balinese houses, including temple areas |
| `batak` | Rumah Bolon from North Sumatra |
| `dayak` | Longhouses of the Dayak people in Kalimantan |
| `javanese` | Javanese traditional houses (Joglo, Kraton) |
| `minangkabau` | Rumah Gadang from West Sumatra |

### Dataset

| Split | Images | Notes |
|-------|--------|-------|
| Train | ~1,754 | 80% stratified split |
| Validation | ~351 | 20% stratified split |
| Test | 444 | Prediction target |

**Severe class imbalance:** `balinese` (~38%) dominates, while `batak` (~5%) and `dayak` (~3%) are severely underrepresented.

---

## Approach: ResNet50 Fine-Tuning with TTA

### Model Architecture

| Component | Choice |
|-----------|--------|
| Backbone | `ResNet50` (IMAGENET1K_V2 pretrained) |
| Head | `Linear(2048, 5)` replacing final FC layer |
| Loss | CrossEntropyLoss with label smoothing ($\epsilon = 0.05$) |
| Input size | 224 × 224 |

### Training Configuration

| Parameter | Value |
|-----------|-------|
| Optimizer | AdamW (lr=3×10⁻⁴, weight_decay=10⁻⁴) |
| Scheduler | CosineAnnealingLR ($T_{max} = 20$) |
| Batch size | 32 |
| Epochs | 20 |
| Mixed precision | FP16 (GradScaler + autocast) |
| Class balancing | WeightedRandomSampler (inverse frequency) |
| Seed | 42 (deterministic) |

### Data Augmentation

**Training:**

- `RandomResizedCrop(224, scale=(0.9, 1.0), ratio=(0.9, 1.1))`
- `RandomHorizontalFlip(p=0.5)`
- `ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2)`

**Validation/Test:**

- `Resize(232)` → `CenterCrop(224)` → `Normalize(ImageNet)`

### Inference

**TTA (Test-Time Augmentation):** average softmax probabilities from original + horizontally flipped image, then argmax.

### Results

| Metric | Value |
|--------|-------|
| **Val Macro F1** | **0.7624** |
| Val Accuracy | 0.8433 |
| Train F1 (last epoch) | 0.9875 |

#### Per-Class Validation Performance

| Class | Precision | Recall | F1 | Support |
|-------|-----------|--------|-----|---------|
| balinese | 0.8834 | 0.9290 | 0.9057 | 155 |
| batak | 0.9091 | 0.5263 | 0.6667 | 19 |
| dayak | 0.6923 | 0.6429 | 0.6667 | 14 |
| javanese | 0.7727 | 0.6800 | 0.7234 | 50 |
| minangkabau | 0.8250 | 0.8761 | 0.8498 | 113 |

---

## Directory Structure

```
Traditional-House-Classification/
├── README.md
├── PROBLEM.md                    # Problem statement (Indonesian)
└── logika-ui-2025.ipynb          # End-to-end notebook
```

---

## Prerequisites

```bash
pip install torch torchvision scikit-learn pandas numpy tqdm
```

Or run directly on **Kaggle GPU** — all dependencies pre-installed.

---

## How to Run

### Option 1: Run on Kaggle (Recommended)

1. Upload `logika-ui-2025.ipynb` to a Kaggle notebook
2. Add the competition dataset as input
3. Set GPU accelerator
4. Run all cells

### Option 2: Run Locally

1. Organize dataset in ImageFolder format:
   ```
   dataset/
   ├── Train/
   │   ├── Train/
   │   │   ├── balinese/
   │   │   ├── batak/
   │   │   ├── dayak/
   │   │   ├── javanese/
   │   │   └── minangkabau/
   └── Test/
       └── Test/
           ├── Test_001.jpg
           ├── ...
           └── Test_444.jpg
   ```
2. Update paths in the notebook
3. Run all cells

### What the Notebook Does

1. **Data Loading** — ImageFolder + stratified 80/20 split
2. **Preprocessing** — Resize, crop, normalize, WeightedRandomSampler
3. **Training** — ResNet50 fine-tune for 20 epochs with early checkpoint
4. **Evaluation** — Macro F1, per-class classification report
5. **Inference** — TTA (original + flipped) → submission CSV

---

## Submission Format

```csv
id,style
Test_001,balinese
Test_002,minangkabau
Test_003,javanese
```

- `id` — test image filename
- `style` — predicted traditional house category
