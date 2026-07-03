# Airbnb Cost Prediction - Problem Statement

## Overview

Seiring berkembangnya industri penyewaan akomodasi jangka pendek secara global, memprediksi harga sewa properti dengan akurat menjadi tantangan penting bagi pemilik properti, platform digital, dan peneliti pasar. Kompetisi Prediksi Harga Sewa Airbnb ini memberikan kesempatan bagi peserta untuk menganalisis berbagai faktor yang memengaruhi penentuan harga listing di berbagai kota dunia.

## Problem Statement
Penentuan harga sewa yang tepat memiliki dampak ekonomi yang signifikan — baik bagi pemilik properti yang ingin memaksimalkan pendapatan, maupun bagi platform yang ingin menjaga daya saing dan kepuasan pengguna.

Peserta ditantang untuk mengembangkan model prediktif yang mampu memperkirakan harga sewa per malam berdasarkan data historis dan atribut listing, termasuk:
- Tipe properti dan kapasitas akomodasi
- Lokasi geografis
- Atribut host
- Data ulasan dan ketersediaan

## Goals
- Mengasah keterampilan membangun model prediksi harga yang akurat dan aplikatif
- Berkontribusi dalam pemahaman mendalam mengenai mekanisme pasar sewa akomodasi
- Menekankan pentingnya pengambilan keputusan berbasis data dalam mengelola portofolio properti

## Timeline
- **Start**: Jul 26, 2025
- **Close**: Aug 2, 2025

## Evaluation
**Metrik**: Root Mean Squared Error (RMSE)

$$\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$$

RMSE dipilih karena memberikan penalti lebih besar pada kesalahan prediksi yang besar, sehingga mendorong model yang tidak hanya akurat secara rata-rata, tetapi juga stabil pada berbagai rentang harga.

```python
import numpy as np
from sklearn.metrics import mean_squared_error

rmse = np.sqrt(mean_squared_error(y_true, y_pred))
```

## Citation
Muhammad Fathur Rizky. SPARTA 2024 Data Science Competition. https://kaggle.com/competitions/sparta-2024-data-science-competition, 2025. Kaggle.

---

## Dataset Description

### Files
| File | Description |
|------|-------------|
| `train.csv` | 300,000 listings dengan semua fitur dan target (`price`) |
| `test.csv` | 100,000 listings tanpa target — prediksi `price` |
| `sample_submission.csv` | Template submisi |

### Key Columns

**Listing Info**: `id`, `name`, `description`, `neighbourhood`, `neighbourhood_cleansed`, `latitude`, `longitude`, `property_type`, `room_type`, `accommodates`, `bathrooms`, `bedrooms`, `beds`, `amenities`, `price`

**Host Info**: `host_id`, `host_name`, `host_since`, `host_location`, `host_response_time`, `host_response_rate`, `host_acceptance_rate`, `host_is_superhost`, `host_listings_count`, `host_verifications`, `host_identity_verified`

**Availability**: `has_availability`, `availability_30/60/90/365`, `availability_eoy`, `estimated_occupancy_l365d`, `estimated_revenue_l365d`

**Reviews**: `number_of_reviews`, `first_review`, `last_review`, `review_scores_*`, `reviews_per_month`

**Other**: `city`

### Data Format
- CSV, UTF-8 encoded
- Dates: ISO format (YYYY-MM-DD)
- Prices: local currency (float)
- Amenities: JSON-formatted string
- Boolean: `t`/`f`
- Missing values present in some columns