# Datasets and Preprocessing

This project used several single-cell RNA sequencing (scRNA-seq) datasets from public repositories to evaluate dropout imputation performance.

## 1. Dataset Overview
- Total of **7 datasets** were selected, each containing between **700–2800 cells** and **1500–3800 genes**.
- Datasets were denoised and standardized before analysis.
- Each dataset included dropout events, which were synthetically masked to simulate missing values.

## 2. Preprocessing Steps
1. Normalization using log-transformed counts.
2. Gene filtering to remove genes expressed in less than 3% of cells.
3. Mask generation to create artificial dropout positions.
4. Split into **training (80%)** and **validation (20%)** sets.

## 3. Evaluation Baselines
Comparative methods included:
- MAGIC
- SAVER
- DCA
- DrImpute (R-based)

These baselines provided benchmarks for assessing the U-Net-based imputation method.
