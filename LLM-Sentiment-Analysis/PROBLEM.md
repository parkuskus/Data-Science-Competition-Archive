# LLM Sentiment Analysis

## Overview

Data Analysis Competition (DAC) 2025 is a university-level data analysis competition across Southeast Asia, held as part of Statistics Fair Week (Pekan Raya Statistika 2025). The competition provides a platform for students to showcase their skills in data analysis, critical thinking, and problem-solving with real-world applications in Business Economics.

**Theme**: "Decoding Consumer Behaviour: Statistical Approaches for Digital Market Insights"

The top 10 teams advance to the final round, held offline at the Department of Statistics, Sepuluh Nopember Institute of Technology (ITS), Indonesia.

## Timeline

| Milestone | Date |
|-----------|------|
| Competition Start | Aug 21, 2025 |
| Competition End | Aug 31, 2025 |

## Problem Statement

As Large Language Models (LLMs) such as GPT, Claude, Gemini, and others continue to grow in popularity, they are increasingly embedded in diverse real-world applications. User reviews have emerged as one of the most direct and unfiltered sources of feedback for developers.

These reviews capture not only surface-level opinions but also subtle sentiment signals that reflect deeper user experiences—such as trust, satisfaction, frustration, or skepticism. For LLM developers, being able to systematically understand and monitor sentiment trends is critical for:

- Identifying strengths and weaknesses across different versions
- Detecting user pain points not visible through quantitative metrics
- Benchmarking against competitors in the LLM ecosystem
- Guiding future improvements in usability, transparency, and ethical considerations

### Challenges

- A single review may contain both positive and negative aspects (mixed/neutral classification)
- Informal writing styles (slang, sarcasm, emojis, abbreviations) introduce ambiguity
- Sentiment distribution is highly imbalanced (satisfied users leave more positive feedback)

## Task

Build an automated sentiment classification system that can:

1. Generalize across multiple LLM platforms (GPT, Claude, Gemini, Grok, Perplexity, DeepSeek)
2. Handle imbalanced data effectively
3. Produce reliable classifications across three sentiment categories:
   - **0** → Negative
   - **1** → Neutral / Mixed
   - **2** → Positive

## Dataset Description

The dataset contains user comments on AI-related applications (GPT, Claude, Gemini, Perplexity, Grok, DeepSeek) in English and Indonesian.

### Files

| File | Description |
|------|-------------|
| `Train/*.csv` | Training set (Jan 2024 – Jul 2025) with Comment, At, AppVersion, Sentiment, source |
| `Test/*.csv` | Test set (same period) with CommentId, Comment, At, AppVersion, source |
| `sample_submission.csv` | Submission format |

### Train Data Distribution

| App | Files |
|-----|-------|
| GPT | `gpt.csv` |
| Claude | `claude.csv` |
| Gemini | `gemini.csv` |
| Grok | `grok.csv` |
| Perplexity | `perplexity.csv` |
| DeepSeek | `deepseek.csv` |

### Sentiment Classes

| Label | Class | Description |
|-------|-------|-------------|
| 0 | Negative | Dissatisfaction, complaints, frustration |
| 1 | Neutral / Mixed | Ambivalent or balanced opinions |
| 2 | Positive | Satisfaction, praise, recommendation |

## Evaluation

### Metric: F1 Score (Macro)

Submissions are evaluated using the F1 score, which balances precision and recall across all three sentiment classes equally.

## Submission Format

```csv
CommentId,Sentiment
gpt_1,2
gpt_2,0
gpt_3,1
...
```

## Analysis Tasks

### Task 1: Comparative Analysis of Negative Reviews

Extract and consolidate negative reviews from multiple LLM platforms. Identify recurring themes using TF-IDF, LDA topic modeling, or similar methods. Compare complaints across platforms and app versions.

### Task 2: Sentiment Analysis Across App Versions

Aggregate sentiment data across multiple LLM versions. Analyze whether specific versions are associated with significant shifts in sentiment using statistical methods (Chi-Square Test, ANOVA). Visualize trends and identify versions requiring investigation.