# Traditional House Classification - Problem Statement

## Overview
Semakin berkurangnya pengetahuan generasi muda tentang rumah adat membuat upaya pelestarian budaya semakin penting. Kompetisi ini mengajak peserta membangun model klasifikasi untuk mengenali berbagai rumah adat Nusantara dari ribuan citra dengan variasi pencahayaan, sudut pandang, dan kualitas gambar.

## Objective
Kurangnya pengetahuan rumah adat Nusantara berisiko menghilangkan identitas budaya dan warisan arsitektur. Peserta diharapkan membangun model yang mengklasifikasikan gambar ke kategori rumah adat yang tepat dengan akurasi dan generalisasi tinggi.

## Goals
- Mengasah keterampilan data science, khususnya computer vision
- Berkontribusi pada pelestarian budaya Nusantara melalui pengenalan dan dokumentasi arsitektur tradisional
- Menekankan pemanfaatan teknologi berbasis data untuk pelestarian warisan budaya di era digital

## Timeline
- **Start**: Sep 21, 2025
- **Close**: Oct 11, 2025

## Evaluation
Submisi dinilai berdasarkan **Macro F1-Score** (rata-rata F1-Score per kelas).

Dipilih karena dataset memiliki distribusi kelas tidak seimbang. Macro F1-Score memberikan bobot sama pada setiap kelas, mendorong model berperforma baik di semua kategori.

## Submission Format
File CSV dengan format:
```csv
id,style
Test_001,balinese
Test_002,minangkabau
Test_003,javanese
...
```
Sesuai `sample_submission.csv` yang disediakan.

## Dataset Description
Setiap gambar memiliki label kategori rumah adat sebagai target klasifikasi.

### Kategori:
| Label | Deskripsi |
|-------|-----------|
| **Javanese** | Rumah tradisional Jawa (Joglo, Kraton) |
| **Balinese** | Rumah tradisional Bali, termasuk area pura |
| **Minangkabau** | Rumah Gadang, Sumatera Barat |
| **Batak** | Rumah Bolon, Sumatera Utara |
| **Dayak** | Rumah panjang, masyarakat Dayak, Kalimantan |

Label terstruktur memudahkan pelatihan model image classification sekaligus mengenal keragaman arsitektur tradisional Indonesia.