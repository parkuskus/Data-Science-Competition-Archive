# Traditional House Classification - Problem Statement

## Overview
As the younger generation's knowledge of traditional houses diminishes, cultural preservation efforts become increasingly important. This competition invites participants to build a classification model capable of recognizing various traditional houses from across Indonesia. The dataset contains thousands of traditional house images from different regions with variations in lighting, viewing angles, and image quality. By applying data science techniques, participants can gain new insights into the visual patterns of Nusantara architecture while contributing to the digitalization and preservation of the nation's cultural heritage.

## Objective
Lack of knowledge about Nusantara traditional houses risks losing cultural identity and valuable architectural heritage. In this competition, participants are expected to build a model that can classify each image into the correct traditional house category with high accuracy and generalization.

## Goals
- Hone skills in data science, particularly computer vision
- Contribute to the preservation of Nusantara culture through recognition and documentation of traditional architecture
- Emphasize the utilization of data-driven technology to support cultural heritage preservation in the digital era

## Timeline
- **Start**: Sep 21, 2025
- **Close**: Oct 11, 2025

## Evaluation
Submissions are evaluated based on **Macro F1-Score**, which represents the average F1-Score per class.

This metric is chosen because the dataset has a highly imbalanced class distribution, making regular accuracy misleading. Macro F1-Score addresses this by giving equal weight to each class, encouraging the model to perform well across all traditional house categories.

## Submission Format
CSV file with the following format:
```csv
id,style
Test_001,balinese
Test_002,minangkabau
Test_003,javanese
...
```
Must match the provided `sample_submission.csv` format.

## Dataset Description
Each image in the dataset is labeled with its traditional house category, used as the target for image classification.

### Categories:

| Label | Description |
|-------|-------------|
| **Javanese** | Javanese traditional houses (Joglo, Kraton) |
| **Balinese** | Balinese traditional houses, including temple areas |
| **Minangkabau** | Rumah Gadang from West Sumatra |
| **Batak** | Rumah Bolon from North Sumatra |
| **Dayak** | Longhouses of the Dayak people in Kalimantan |

The clear and structured labels make it easier to train and test image classification models while introducing the diversity of Indonesian traditional architecture.