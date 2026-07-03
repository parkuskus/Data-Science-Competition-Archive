# Football Score Prediction - Problem Statement

## Overview
**Yooji and Mina's World Cup Journey**

My name is Yooji, and beside me is Mina — a girl whose smile has always been the reason I keep going. This year, 2026, should have been the peak of our happiness. After years of saving, I could finally bring Mina to America to fulfill her dream of watching the World Cup. Inside the calm airplane cabin, we sat leaning together, sharing a pair of earphones, watching a movie about a swarm of bees sabotaging a flight. I still remember the warmth of her hand holding mine, before a strange light from the monitor screen struck and took everything away in an instant.

I could only stand frozen as Mina's body dissolved into particles of light and was sucked into the monitor screen right before my eyes. The next moment, Mina's sweet face vanished, replaced by an cold and arrogant figure named Beeta, the Bee King ruler of the data dimension. Beeta sneered from behind the glass, her voice buzzing in my head, declaring that Mina was now her prisoner in a complex digital prison. She would only release Mina if I could prove that the future of football was not a random mystery, but a pattern I could conquer with certainty.

Now, Mina's life was entirely at my fingertips. Beeta gave me a massive "data book" containing match histories and player statistics, then challenged me to predict the scores of the entire tournament with perfect accuracy. If my predictions were wrong, Mina would be forever erased from the real world, her soul dissolved into dead numbers in Beeta's digital hive. Inside the airplane cabin, now silent and frozen in time, I had to race against the seconds to craft the most accurate predictions. I had to bring Mina back, and I needed your help to win this life-or-death game before the final whistle was blown.

## Evaluation
**Metric**: Augmented Weighted Mean Absolute Error (AW-MAE)

Unlike regular MAE, this metric also considers:
- Exact score accuracy
- Match outcome accuracy (win / draw / loss)
- Goal difference accuracy
- Tournament importance level

Lower AW-MAE values indicate better model performance.

### 1. Base Error (MAE)

$$\text{MAE} = \frac{|\text{teamgoals}_{true} - \text{teamgoals}_{pred}| + |\text{oppgoals}_{true} - \text{oppgoals}_{pred}|}{2}$$

### 2. Penalty Components

$$\text{Penalty} = 0.3(1-\text{Exact}) + 0.25(1-\text{Outcome}) + 0.15(1-\text{GD})$$

| Penalty | Weight | Condition |
|---------|--------|-----------|
| Exact Score | 0.30 | Predicted score doesn't match exactly |
| Outcome | 0.25 | Predicted win/draw/loss is wrong |
| Goal Difference | 0.15 | Predicted goal difference is wrong |

### 3. Outcome Multiplier

- **1.0** if outcome is correct
- **1.5** if outcome is wrong

### 4. Non-Linear Scaling

$$\text{Loss} = (\text{RawLoss} \cdot \text{Multiplier})^{1.5}$$

### 5. Tournament Weighting

| Tournament | Weight |
|------------|--------|
| FIFA World Cup | 2.00 |
| AFC Asian Cup | 1.80 |
| UEFA Euro | 1.80 |
| Copa America | 1.80 |
| Africa Cup of Nations | 1.80 |
| CONCACAF Gold Cup | 1.70 |
| Qualification matches | 1.60 |
| Friendly | 0.96 |
| Default | 1.20 |

## Submission Format

```csv
Id,team_goals,opp_goals
M000001_Seychelles,0,0
M000001_Mauritius,0,0
M000002_Argentine,4,2
M000002_Brazil,2,4
```

- Each match has two rows (one per team)
- `team_goals` and `opp_goals` must be non-negative integers

---

## Dataset Description

### Files
| File | Description |
|------|-------------|
| `train.csv` | 78,772 rows with all features and targets |
| `test.csv` | 42,422 rows without targets |
| `sample_submission.csv` | Submission template |

### Columns

**Identity & Basic Info**: `Id`, `match_id`, `date`, `gender`, `team`, `opponent`

**Match Conditions**: `is_home`, `neutral`, `tournament`, `venue_country`, `confederation_team`, `confederation_opp`

**Performance Metrics** (train only): `elo_team`, `elo_opponent`, `rank_team`, `rank_opponent`, `team_points_last5/last10`, `team_gd_last5`, `team_win_rate_last10`, `h2h_points_last5`, `days_since_last_match`

**Geographic & Socio-Economic Data**: `population_team/opp`, `gdp_per_capita_team/opp`, `altitude_venue`, `distance_travel_team/opp`, `temperature_venue`

**Target Variables**: `team_goals`, `opp_goals`

### Important Note
The test set **does not have** historical features (ELO, rank, form) — these must be reconstructed from the training data.