# Model Evaluation and Analysis

This section summarizes how the modified U-Net model was evaluated for dropout imputation in single-cell RNA-seq data and reflects on key findings beyond the published results.

---

## 1. Evaluation Metrics

The model performance was assessed using the following metrics:
- **Mean Squared Error (MSE):** Measures the reconstruction error between imputed and ground-truth values.  
- **Pearson Correlation Coefficient (PCC):** Evaluates the consistency of gene expression patterns after imputation.  
- **Adjusted Rand Index (ARI):** Assesses how well the imputed data preserves cell clustering structure.

---

## 2. Comparative Results

The proposed method was compared against several established tools:
- **MAGIC**
- **SAVER**
- **DCA**
- **DrImpute**

Across all datasets, the modified U-Net achieved **lower MSE** and **higher ARI**, suggesting better recovery of biological variation and reduced over-smoothing.

You may include a results figure here:  
`![Comparison of methods](../figures/results_comparison.png)`

---

## 3. Observations and Insights

- The **U-Net’s partial convolution layers** significantly improved handling of sparse dropout regions.  
- Performance gains were especially clear in datasets with **low sequencing depth**, where missing values were frequent.  
- Traditional matrix completion methods (e.g., SVD, NMF) struggled with complex gene-gene dependencies, while U-Net captured these patterns more effectively.

---

## 4. Limitations and Reflections

While the model demonstrated strong quantitative results, several challenges remain:
- Limited interpretability of deep learning–based imputations compared to statistical models.  
- Potential overfitting in small-cell datasets despite early stopping.  
- Future work could involve **hybrid models** that combine statistical priors with deep architectures for more robust biological interpretability.
