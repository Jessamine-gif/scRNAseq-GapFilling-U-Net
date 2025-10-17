# scRNAseq-GapFilling-U-Net

This repository implements a **U-Net–based model** for single-cell RNA sequencing (scRNA-seq) data gap filling.  
The model treats each gene expression profile as a grayscale matrix and performs image-style reconstruction to restore missing values, improving data completeness and clustering quality.

---

## 1. Overview

Single-cell RNA sequencing data are often affected by dropout events that introduce missing values.  
To address this issue, this project applies a **modified U-Net** architecture that integrates random masking, fine-tuned loss optimization, and a smooth reconstruction strategy to recover accurate expression levels.

Key features include:
- Transformation of gene expression matrices into 2D “image-like” representations.  
- Random masking to simulate dropout patterns.  
- A U-Net–like model trained to minimize reconstruction loss.  
- Recovery of reconstructed matrices back to gene-level expression.

---

## 2. Visual Summary

### Data Processing Flow
![Flowchart](figures/Single-cell%20sequencing%20data%20gap-filling%20algorithm%20flow%20chart.png)

### Model Structure
![Architecture](figures/Variable%20combination%20algorithm%20interpolation%20flow.png)

### Result Comparison
![Results](figures/Comparison%20of%20the%20clustering%20effect%20before%20and%20after%20filling%20the%20gap.png)


---

## 3. Model Description

### 3.1 Architecture
The model is derived from the classical U-Net structure, consisting of:
- **Encoder:** Extracts contextual and spatial dependencies among genes.  
- **Bottleneck:** Learns latent representations of gene–gene relationships.  
- **Decoder:** Reconstructs masked expression patterns.  
- **Skip connections:** Preserve local continuity between gene subsets.  

### 3.2 Training Configuration
- Dynamic random masks simulate different dropout distributions.  
- The training objective combines multiple reconstruction losses:

  \[
  L = \alpha L_{MSE} + \beta L_{MAE} + \gamma L_{Smooth}
  \]

| Parameter | Setting |
|------------|----------|
| Optimizer | Adam |
| Learning rate | 1e-3 |
| Batch size | 16 |
| Epochs | 100 |
| Loss function | Combined MSE + Regularization |
| Activation | ReLU (Encoder), Sigmoid (Decoder) |
| Dropout | 0.2 |
| Framework | TensorFlow / PyTorch |

---

## 4. Evaluation

The model performance is evaluated using the following metrics:

| Metric | Description |
|--------|--------------|
| Adjusted Rand Index (ARI) | Evaluates clustering consistency before and after imputation |
| RMSE | Quantifies reconstruction error |
| Pearson correlation | Measures the preservation of biological correlations |

The proposed U-Net–based approach demonstrates higher reconstruction fidelity and improved clustering stability compared to traditional imputation algorithms.

---

## 5. Citation

If you use this repository in your research, please cite:

> Jiang, Y. (2025). *scRNAseq-GapFilling-U-Net: A Modified U-Net Architecture for Single-Cell Data Imputation.*

---

## 6. License

This project is distributed under the MIT License.
