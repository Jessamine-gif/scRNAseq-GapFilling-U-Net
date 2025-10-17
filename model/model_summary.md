# Model Summary: Modified U-Net for scRNA-seq Gap Filling

## 1. Overview
This project applies a **modified U-Net architecture** to perform gap filling in single-cell RNA sequencing (scRNA-seq) data.  
The approach treats each cell’s gene expression profile as a 2D grayscale matrix, enabling image-based modeling of expression recovery tasks.

Unlike traditional imputation algorithms, this method incorporates **random masking**, **fine-tuned loss optimization**, and a **bi-level encoder–decoder design**, effectively enhancing data completeness and clustering stability.

---

## 2. Model Design

### 2.1 Core Architecture
The model is derived from the classical U-Net, composed of:
- **Encoder path** — captures spatial and contextual dependencies in the gene expression matrix.
- **Bottleneck layer** — learns latent biological feature representations.
- **Decoder path** — reconstructs masked gene values based on encoded features.
- **Skip connections** — preserve local relationships between genes and prevent over-smoothing.

### 2.2 Input Representation
- Each cell is transformed into an *image-shaped matrix*, where  
  - Rows represent **genes**,  
  - Columns represent **expression levels (log-normalized)**.  
- Random masks are applied to simulate missing data patterns.

### 2.3 Training Objective
The network minimizes a combination of reconstruction losses:
\[
L = \alpha L_{MSE} + \beta L_{MAE} + \gamma L_{Smooth}
\]
where:
- \( L_{MSE} \): measures reconstruction accuracy of masked entries.  
- \( L_{MAE} \): reduces bias toward large-value genes.  
- \( L_{Smooth} \): enforces smooth transitions across neighboring gene regions.  
Coefficients \( \alpha, \beta, \gamma \) are tuned empirically.

---

## 3. Training Configuration

| Parameter | Value / Setting |
|------------|----------------|
| Optimizer | Adam |
| Learning Rate | 1e-3 |
| Batch Size | 16 |
| Epochs | 100 |
| Loss Function | Combined MSE + Regularization |
| Activation | ReLU (Encoder), Sigmoid (Decoder) |
| Dropout | 0.2 |
| Framework | TensorFlow / PyTorch |

During training, **random masks** are dynamically generated to ensure robust recovery against different missing data distributions.

---

## 4. Evaluation Metrics

The model’s performance is assessed using:
- **ARI (Adjusted Rand Index)** — evaluates clustering consistency before and after imputation.  
- **RMSE (Root Mean Square Error)** — quantifies reconstruction accuracy.  
- **Pearson correlation** — measures the fidelity of recovered gene–gene relationships.

Empirically, the U-Net-based gap-filling model achieves higher data smoothness and improved biological clustering compared with baseline imputation methods.

---

## 5. Key Insights
- Treating gene expression as structured image data enables powerful spatial modeling.  
- The combination of **random masking + U-Net reconstruction** effectively restores missing values.  
- The architecture generalizes well across datasets, supporting diverse scRNA-seq modalities.  

---

*This summary consolidates model architecture, parameter configurations, and evaluation logic for reproducibility and transparency.*
