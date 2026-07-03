# Airbnb Cost Prediction - Problem Statement

## Overview
**Source**: BukitVista

As the short-term rental accommodation industry grows globally, accurately predicting property rental prices has become a significant challenge for property owners, digital platforms, and market researchers. This Airbnb Rental Price Prediction competition provides an opportunity for participants to analyze various factors influencing listing prices across cities worldwide.

## Problem Statement
Setting the right rental price has significant economic impact — both for property owners looking to maximize revenue and for platforms aiming to maintain competitiveness and user satisfaction.

Participants are challenged to develop a predictive model capable of estimating nightly rental prices based on historical data and listing attributes, including:
- Property type and accommodation capacity
- Geographic location
- Host attributes
- Review and availability data

## Goals
- Hone skills in building accurate and applicable price prediction models
- Contribute to deeper understanding of rental market dynamics
- Emphasize data-driven decision making in property portfolio management

## Timeline
- **Start**: Jul 26, 2025
- **Close**: Aug 2, 2025

## Evaluation
**Metric**: Root Mean Squared Error (RMSE)

$$\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$$

RMSE is chosen because it imposes larger penalties on bigger prediction errors, encouraging models that are not only accurate on average but also stable across various price ranges.

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
| `train.csv` | 300,000 listings with all features and target (`price`) |
| `test.csv` | 100,000 listings without target — predict `price` |
| `sample_submission.csv` | Submission template |

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