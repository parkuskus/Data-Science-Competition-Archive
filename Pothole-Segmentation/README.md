# Pothole Segmentation

> **Competition:** Data Science ARA 7.0 — Kaggle
> **Metric:** Dice Coefficient | **Team:** GakKuliah

---

## Problem Overview

Road surface monitoring relies heavily on manual inspection and public complaints, making pothole identification slow and unevenly distributed. This makes it difficult to prioritize repairs and allocate resources effectively.

This project addresses the **Data Science ARA 7.0** Kaggle competition, where the task is to build an **image segmentation model** that identifies and maps pothole areas at the pixel level from road surface images. The evaluation metric is the **Dice Coefficient**, measuring overlap between predicted and ground-truth masks.

### Dataset

| Split | Images | Notes |
|-------|--------|-------|
| Train | 498 | Paired with ground-truth masks |
| Test | 295 | Prediction target |

- Foreground pixel ratio: **8.39%** (severe class imbalance)
- Pothole area range: 0.02% – 67.4% of image
- Mean pothole area: 13.49%
- Object count per image: 1 – 67
- No empty masks (every image contains pothole)

---

## Approach

### Model: MAnet (Multi-scale Attention Network)

| Component | Choice |
|-----------|--------|
| Encoder | `mit_b3` (EfficientNet-like, timm pretrained) |
| Decoder | MAnet with Position Attention Block (PAB), 64 channels |
| Output | 1 class (binary segmentation) |
| Input size | 640 × 640 |

Chosen for its attention mechanism that distinguishes road features from noise while preserving global asphalt texture context.

### Loss Functions (3 Best Variants)

| Variant | Loss Composition |
|---------|-----------------|
| **Best 1** | 0.35 × DiceLoss + 0.50 × TverskyLoss(α=0.3, β=0.7) + 0.15 × FocalLoss(α=0.25, γ=2.0) |
| **Best 2** | 0.40 × DiceLoss + 0.60 × TverskyLoss(α=0.3, β=0.7) |
| **Best 3** | 0.60 × DiceLoss + 0.40 × FocalLoss(α=0.5, γ=2.0) |

### Training Configuration

| Parameter | Value |
|-----------|-------|
| Optimizer | AdamW (lr=6e-5, weight_decay=1e-3) |
| Scheduler | CosineAnnealingWarmRestarts (T_0=5, T_mult=2, η_min=1e-6) |
| Batch size | 4 |
| Epochs | 25 – 28 |
| Mixed precision | FP16 (autocast + GradScaler) |
| Gradient clipping | max_norm=1.0 (Best 1 only) |

### Inference Pipeline

1. **Multi-scale TTA** — scales [0.75, 1.0, 1.25, 1.5] × 2 flips = 8 predictions averaged (Best 1)
2. **Adaptive thresholding** — brightness-based threshold selection (0.40 – 0.55)
3. **Post-processing** — morphological closing (ellipse 3×3) + connected component filtering (remove small regions)

### Results

| Variant | Dice Score |
|---------|-----------|
| Best 1 | **0.76442** |
| Best 2 | 0.76397 |
| Best 3 | 0.75973 |

---

## Directory Structure

```
Pothole-Segmentation/
├── README.md
├── DatSciARA7_GakKuliah.ipynb         # End-to-end notebook (EDA → Training → Submission)
├── pipeline_summary.md                # Pipeline documentation (Indonesian)
├── sample_submission.csv              # Submission template (RLE format)
│
└── dataset/
    ├── train/
    │   ├── images/                    # 498 training images (.jpg)
    │   └── mask/                      # 498 ground-truth masks (.png)
    └── test/
        └── images/                    # 295 test images (.jpg)
```

---

## Prerequisites

```bash
pip install torch torchvision segmentation-models-pytorch timm albumentations opencv-python numpy pandas scikit-learn scikit-image matplotlib tqdm
```

Or run directly on **Kaggle GPU** (P100/T4) — all dependencies pre-installed.

---

## How to Run

### Option 1: Run on Kaggle (Recommended)

1. Upload `DatSciARA7_GakKuliah.ipynb` to a Kaggle notebook
2. Add the competition dataset as input
3. Set GPU accelerator (P100 or T4)
4. Update dataset paths in the notebook to match Kaggle input paths:
   ```python
   TRAIN_IMG_DIR = "/kaggle/input/data-science-ara-7-0/dataset/train/images"
   TRAIN_MASK_DIR = "/kaggle/input/data-science-ara-7-0/dataset/train/mask"
   TEST_IMG_DIR = "/kaggle/input/data-science-ara-7-0/dataset/test/images"
   ```
5. Run all cells

### Option 2: Run Locally

1. Place dataset in `dataset/` following the directory structure above
2. Update paths in the notebook:
   ```python
   TRAIN_IMG_DIR = "dataset/train/images"
   TRAIN_MASK_DIR = "dataset/train/mask"
   TEST_IMG_DIR = "dataset/test/images"
   OUTPUT_DIR = "output/"
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt  # or install manually
   ```
4. Run all cells in `DatSciARA7_GakKuliah.ipynb`

### What the Notebook Does

1. **EDA** — Dataset statistics, class distribution, brightness/contrast/sharpness analysis
2. **Preprocessing** — Resize to 640×640, mask binarization, 90/10 stratified split
3. **Augmentation** — Flip, perspective, grid distortion, color jitter, shadow (Albumentations)
4. **Training** — 3 loss variants trained independently with MAnet
5. **Inference** — Multi-scale TTA + adaptive thresholding + morphological post-processing
6. **Submission** — RLE-encoded masks saved as CSV

---

## Submission Format

```csv
ImageId,rle
test_001.jpg,10000 11 39200 7
test_002.jpg,
test_003.jpg,501 3 900 10
```

- `ImageId` — test image filename
- `rle` — RLE-encoded predicted mask (empty if no pothole)

### RLE Encoding

```python
def encode_rle(mask: np.ndarray, pos_value: int = 255) -> str:
    binary = (mask == pos_value).astype(np.uint8)
    pixels = binary.T.flatten()
    pixels = np.concatenate([[0], pixels, [0]])
    runs = np.where(pixels[1:] != pixels[:-1])[0] + 1
    runs[1::2] -= runs[0::2]
    return " ".join(str(x) for x in runs)
```
