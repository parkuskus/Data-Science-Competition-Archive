# README.md Style Guide

> This document defines the standard format for writing `README.md` files in the Data Science Competition Archive. Follow these rules to ensure consistency across all project folders.

---

## Template Structure

```markdown
# {Project Title}

> **Competition**: {Competition Name} — {Platform/Year}
> **Source**: {Data Source} (optional)
> **Metric**: {Evaluation Metric} | **Team**: {Team Name} (optional)

---

## Problem Overview

{1-3 sentences describing the competition task, dataset characteristics, and key challenges.}

### Classes (for classification) / Dataset (for other tasks)

| {Column} | {Column} |
|----------|----------|
| ... | ... |

---

## Approach: {Model/Method Name}

### Data Preprocessing

| Step | Method |
|------|--------|
| ... | ... |

### Feature Engineering (if applicable)

| Feature Type | Method |
|--------------|--------|
| ... | ... |

### Model Configuration

| Parameter | Value |
|-----------|-------|
| ... | ... |

### Training Strategy (optional)

- **Key point 1**: Details
- **Key point 2**: Details

---

## Results

| Metric | Value |
|--------|-------|
| ... | ... |

### Key Insights (optional)

- Insight 1
- Insight 2

---

## Directory Structure

```
{Project-Folder}/
├── README.md
├── PROBLEM.md
├── {notebook}.ipynb
└── {other files}
```

---

## Prerequisites

```bash
pip install {packages}
```

---

## How to Run

1. {Step 1}
2. {Step 2}
3. {Step 3}

### What the Notebook Does (optional)

1. **{Step Name}** — {Description}
2. **{Step Name}** — {Description}

---

## Submission Format

```csv
{header}
{example row 1}
{example row 2}
```

- `{column}` — {description}
```

---

## Section Rules

### 1. Title (`#`)
- Use `# {Project Title}`
- Title should match the folder name (with spaces instead of hyphens)
- Use Title Case

### 2. Metadata Block (`>`)
- Always start with a blockquote containing key competition info
- Required fields: `Competition`, `Metric`
- Optional fields: `Source`, `Team`
- Use `|` to separate multiple fields on the same line
- Use `**bold**` for field names

Example:
```markdown
> **Competition**: SPARTA 2024 Data Science Competition
> **Source**: BukitVista (Kaggle)
> **Metric**: RMSE | **Team**: GakKuliah
```

### 3. Horizontal Rules (`---`)
- Place `---` between major sections
- Always after metadata block
- Between Approach and Results
- Between Results and Directory Structure
- Between Directory Structure and How to Run

### 4. Problem Overview
- Write 1-3 sentences maximum
- Describe: task, dataset size/characteristics, key challenge
- Can include a table for classes/dataset splits if relevant
- Mention class imbalance if present

### 5. Approach Section
- Start with `## Approach: {Model/Method Name}`
- Use `###` for subsections
- Always use tables for configuration (Preprocessing, Model Config, etc.)
- Tables should have clear headers: `Parameter | Value` or `Step | Method`

**Subsection order:**
1. Data Preprocessing (always)
2. Feature Engineering (if applicable)
3. Model Architecture (for deep learning)
4. Model Configuration (always)
5. Training Strategy (optional)
6. Data Augmentation (for computer vision)

### 6. Results Section
- Use `## Results` or `## Results Summary`
- Always include a metrics table
- Bold the best/primary metric
- Add Key Insights subsection if there are notable observations
- For classification: include per-class performance table

### 7. Directory Structure
- Use code block with tree-style formatting
- Include all important files
- Add inline comments for non-obvious files
- Use `├──` and `└──` for proper tree display

### 8. Prerequisites
- Use `pip install` command in bash code block
- List all required packages
- Mention alternative platforms (e.g., Kaggle GPU) if applicable

### 9. How to Run
- Use numbered list for steps
- Keep steps minimal (3-5 steps max)
- Include "What the Notebook Does" subsection for complex pipelines
- Number notebook steps to show pipeline flow

### 10. Submission Format
- Show CSV header + 2-3 example rows
- Explain each column with bullet points below
- Match the actual `sample_submission.csv` format

---

## Formatting Rules

1. **Tables**: Always use Markdown tables for structured data
2. **Code**: Use backticks for file names, functions, parameters
3. **Bold**: Use for emphasis on key metrics, best results, important terms
4. **Language**: Use English for all README files
5. **Length**: Keep README concise (100-200 lines max)
6. **No emojis**: Avoid emojis unless part of the competition theme

---

## Quick Reference: Section Checklist

- [ ] Title with project name
- [ ] Metadata blockquote (Competition, Metric, optional: Source, Team)
- [ ] Horizontal rule
- [ ] Problem Overview (1-3 sentences)
- [ ] Approach section with subsections
- [ ] Results with metrics table
- [ ] Directory Structure
- [ ] Prerequisites (pip install)
- [ ] How to Run (numbered steps)
- [ ] Submission Format (CSV example)