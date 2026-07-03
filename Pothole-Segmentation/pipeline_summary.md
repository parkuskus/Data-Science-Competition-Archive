# Summary Pipeline Notebook

Notebook ini berisi pipeline segmentasi pothole end-to-end, mulai dari eksplorasi data, pelatihan model, sampai pembuatan submission.

## 1. EDA dan Analisis Dataset

Notebook dimulai dengan analisis dataset untuk memahami karakter data train dan test. Analisis yang dilakukan meliputi:

- jumlah image dan mask
- distribusi luas pothole per gambar
- distribusi ukuran gambar
- analisis brightness dan contrast
- analisis sharpness atau blur
- distribusi spasial pothole
- jumlah objek pothole per gambar
- deteksi potential domain shift antara train dan test

Dari EDA, notebook menyimpulkan bahwa dataset sangat imbalanced secara piksel, variasi ukuran objek sangat besar, dan pothole cenderung muncul di area tengah gambar.

## 2. Data Preparation

Data train diambil dari folder image dan mask, lalu dipasangkan berdasarkan nama file. Setelah itu data dibagi menjadi train dan validation menggunakan `train_test_split`.

Semua image di-resize ke ukuran tetap 640 x 640 agar input ke model konsisten. Pada tahap dataset loader, mask diubah menjadi binary mask untuk kebutuhan segmentasi.

## 3. Augmentasi

Notebook menggunakan augmentasi yang cukup agresif untuk meningkatkan generalisasi model, seperti:

- horizontal flip
- vertical flip
- perspective transform
- grid distortion
- CLAHE
- random gamma
- color jitter
- random shadow

Tujuannya adalah membuat model lebih robust terhadap variasi kondisi jalan, pencahayaan, dan bentuk objek.

## 4. Model Training

Model utama yang dipakai adalah **MANet** dengan encoder `mit_b3` dan pretrained weight ImageNet.

Loss function dirancang untuk mengatasi class imbalance pada segmentasi, dengan variasi berikut di beberapa eksperimen:

- Dice Loss
- Tversky Loss
- Focal Loss

Optimisasi dilakukan dengan:

- AdamW optimizer
- CosineAnnealingWarmRestarts scheduler
- mixed precision training menggunakan `autocast` dan `GradScaler`

Evaluasi validation dilakukan memakai Dice Score, dan model terbaik disimpan berdasarkan skor validation tertinggi.

## 5. Variasi Pipeline Terbaik

Notebook menyimpan beberapa varian pipeline terbaik:

### Best 1

- loss: Dice + Tversky + Focal
- inference memakai multi-scale TTA
- threshold adaptif berdasarkan brightness gambar
- post-processing dengan morphological closing dan filtering berdasarkan area minimum

### Best 2

- loss: Dice + Tversky
- pipeline training lebih sederhana
- inference tetap memakai TTA dan post-processing area filtering

### Best 3

- menyimpan top-3 checkpoint terbaik
- melakukan averaging bobot model
- inference memakai TTA, threshold adaptif, dan filtering komponen kecil

## 6. Inference dan Post-processing

Pada tahap prediksi test, notebook menggunakan:

- multi-scale TTA
- horizontal flip TTA
- threshold adaptif
- morphological closing
- connected component filtering
- encoding hasil mask ke format RLE

Output akhir disimpan sebagai file submission CSV.

## 7. Kesimpulan

Secara keseluruhan, pipeline notebook ini adalah:

1. memahami karakteristik dataset melalui EDA,
2. menyiapkan data dengan resize, split, dan augmentasi,
3. melatih model segmentasi MANet,
4. mengevaluasi dengan Dice Score,
5. melakukan inference dengan TTA dan post-processing,
6. menghasilkan submission dalam format RLE.

Pipeline ini dirancang untuk menangani dataset pothole yang bervariasi, imbalanced, dan memiliki potensi bias spasial.