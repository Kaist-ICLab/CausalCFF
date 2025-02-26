# CausalCFF

**Causal Analysis between User Stress Level and Contextually Filtered Features Extracted from Mobile Sensor Data**

## Table of Contents
1. [Introduction](#introduction)
2. [Paper Summary](#paper-summary)
3. [Repository Overview](#repository-overview)
4. [Data Description](#data-description)
5. [Code Structure & Workflow](#code-structure--workflow)
    1. [Exploratory Data Analysis](#1-exploratory-data-analysis-eda)
    2. [Feature Engineering](#2-feature-engineering)
    3. [Data Resampling](#3-data-resampling)
    4. [Rule Generation & Feature Extraction](#4-rule-generation--feature-extraction)
    5. [Aggregation & Normalization](#5-aggregation--normalization)
6. [Results](#results)
7. [How to Cite](#how-to-cite)

---

## Introduction
CausalCFF is a framework designed to analyze how high-level contextual features from mobile sensor data influence user stress levels. By applying causal inference methods (specifically Convergent Cross Mapping or CCM), the project identifies potential stress triggers that go beyond simple correlations. This repository contains all relevant code to reproduce the analysis, including data preprocessing, feature extraction, and causal inference steps.

---

## Paper Summary
This repository accompanies the paper: **“CausalCFF – Causal Analysis between User Stress Level and Contextually Filtered Features Extracted from Mobile Sensor Data.”**

### Key Points:
- **Motivation & Background:** Stress is a key factor in mental well-being. Traditional sensor-based studies often examine single-sensor features, missing the influence of multi-sensor contexts.
- **Methodology:** 
  1. **Contextual Feature Extraction:** Using association rule mining on multi-modal sensor data (location, app usage, physical activity) to extract high-level behavioral patterns.
  2. **Causal Inference (CCM):** Determining causal relationships between these contextually filtered features (CFFs) and self-reported stress levels.
- **Results & Implications:** 
  - Identified top behavior patterns (e.g., frequent workplace visits with low home time) as potential stressors.
  - Emphasized personalized interventions since causal strengths vary across individuals.
  - Suggested future work on sequential rule mining and improved interpretability of complex features.

---

## Repository Overview
The repository is structured around Jupyter notebooks and scripts that:
1. **Load and preprocess the dataset.**
2. **Extract high-level contextually filtered features (CFFs).**
3. **Perform causal analysis using Convergent Cross Mapping (CCM).**
4. **Produce interpretable metrics and visualizations for stress analysis.**

You can use this code to replicate the entire pipeline—from data preparation to final causal results.

---

## Data Description
- **Original Dataset**: An open-source dataset with mobile sensor data and stress self-reports from 24 university students over six weeks (https://github.com/Kaist-ICLab/DeepStress_Dataset).
  - **GPS Data**: Categorized into home, work, or other places.
  - **Physical Activity**: Walking, running, sitting, etc.
  - **App Usage**: Grouped into categories like social media, productivity, entertainment.
  - **Self-Reported Stress**: 5-point Likert scale.

> **Note**: This repository assumes you already have access to the dataset in the correct format (e.g., `combined.csv`, `proc_interpretable_updated.pkl`). For privacy and licensing reasons, the dataset may not be included in this repository. Please refer to the dataset’s license and usage conditions in the original dataset page.

---

## Code Structure & Workflow

### 1. Exploratory Data Analysis (EDA)
- **Notebook Sections**:  
  - Markdown cells describing the dataset and the rationale behind EDA.  
  - Binarizing stress labels and other label processing steps.  
- **Key Operations**:  
  1. Load `combined.csv` and parse timestamps.  
  2. Plot distribution of stress levels across all participants.  
  3. Assign [uid, timestamp] as the primary index for subsequent analyses.

### 2. Feature Engineering
- **Notebook Sections**:  
  - Introduction to high-level features derived from sensor data.  
  - Markdown explaining how the dataset is re-organized by participant code (pcode).  
- **Key Operations**:
  1. Load `proc_interpretable_updated.pkl`.  
  2. Map sensor data to each participant (P01, P02, …).  
  3. Remove or exclude certain data types (e.g., `APP_DUR_UNKNOWN`, `LOC_DUR_others`) to focus on interpretable features.

### 3. Data Resampling
- **Notebook Sections**:
  - Discussion on resampling sensor data at a 1-second interval.  
  - Use of Ray for parallel processing.  
- **Key Operations**:
  1. Identify unique user IDs.  
  2. Create a dictionary for categorical feature distributions.  
  3. Resample sensor readings, removing duplicates for efficiency.

### 4. Rule Generation & Feature Extraction
- **Notebook Sections**:
  - Association rule mining to generate contextually filtered features (CFFs).  
  - Explanation of sub-features based on time windows prior to each stress measurement (ESM response).
- **Key Operations**:
  1. Define window sizes (e.g., 160 minutes) and sub-windows (e.g., 8).  
  2. Use `extract_extended_parallel()` to generate extended features for each time window.  
  3. Save the extracted CFFs (e.g., in `Features/arm/` directory).

### 5. Aggregation & Normalization
- **Notebook Sections**:
  - Aggregating sub-features into final feature vectors for each participant.  
  - Handling missing data and normalization methods.  
- **Key Operations**:
  1. Load the sub-feature file (e.g., `subfeature_160MIN_8.csv`).  
  2. Check for missing values and decide on an imputation strategy.  
  3. Finalize the aggregated feature set to be fed into the causal inference module.

---

## Results
Upon successful execution of the notebooks, you will obtain:
- **Top Contextually Filtered Features (CFFs)** that have a statistically significant causal relationship with stress levels (p < 0.05).
- **Visualizations & Summary Tables** indicating personalized variations in causal strengths.
- **Insights into Behavioral Patterns**, highlighting the importance of context-aware stress management strategies.

---

## How to Cite
```
Panyu Zhang, Gyuwon Jung, Uzair Ahmed, and Uichin Lee. 2025. Causal-CFF: Causal Analysis between User Stress Level and Contextually Filtered Features Extracted from Mobile Sensor Data. In Extended Abstracts of the 2025 CHI Conference on Human Factors in Computing Systems (CHI EA ’25), April 26–May 01, 2025, Yokohama, Japan. ACM, New York, NY, USA, 6 pages. https://doi.org/10.1145/3706599.3719776
```

**Questions?**
Feel free to open an issue or reach out with any questions!
