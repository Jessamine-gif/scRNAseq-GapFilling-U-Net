# Design Decisions and Notes

These are my working notes for  
**“Variable Combinatorial Gap-Filling Method for Single-Cell RNA-Seq Data” (AMNS, 2024).**  
This file does not repeat the paper’s results. It summarizes how I approached the problem,  
the decisions made during development, and what I learned from each stage.

---

## 1 Framing the Problem

The goal was to correct dropouts in scRNA-seq data while keeping true zeros unchanged.  
I treated this task as an **image inpainting problem**: the expression matrix was represented  
as a grayscale image, where missing entries corresponded to blank pixels.  
This formulation allowed the use of computer vision models such as U-Net.

Alternative approaches considered:
- **Matrix completion (SVD, NMF):** too linear and unable to capture local nonlinear patterns.  
- **Denoising autoencoders:** stable but performed poorly under high sparsity (>80%).  

---

## 2 Mask Generation

At first, I used a fixed 10% random mask to simulate missing data.  
The model converged quickly but overfitted to that single dropout pattern.  
I then switched to **variable dropout rates (5–50%)** per batch to increase diversity.  

Observations:
- Greater diversity in mask rates improved generalization.  
- Dropout rates above 60% caused unstable training, likely due to insufficient valid information.  

**Lesson:** mask diversity mattered more than mask geometry.

---

## 3 Architecture Choice

A **modified U-Net with partial convolution layers** was adopted.  
Each layer updated its own mask, helping valid signals propagate while avoiding  
the mixing of true zeros into convolutional statistics.

Discarded alternatives:
- **Full convolution networks:** introduced zero contamination.  
- **GAN-based models:** generated visually realistic results but lacked biological meaning.  
- **Residual stacks:** efficient, but weak in restoring large dropout regions.  

---

## 4 Loss Function Design

To balance numerical precision and structural consistency,  
the loss combined three components:
- **Pixel loss (L1):** preserves numeric accuracy.  
- **Perceptual loss:** maintains structural similarity between clusters.  
- **Style loss:** prevents over-smoothing and keeps texture patterns.  

Empirically, the best ratio was **1 : 0.5 : 0.2**.  
When the style weight exceeded 0.5, the output became unstable and “over-textured.”

---

## 5 Observations and Interpretations

The combination of **variable masks** and **partial convolutions** performed best  
for continuous missing regions.  
Using only pixel loss improved numeric values but not clustering quality.  
Adding perceptual and style losses made **cell-type boundaries more distinct**.  
When the number of highly variable genes exceeded 2000, ARI increased slightly  
but with much higher computational cost — the trade-off wasn’t worthwhile.  

---

## 6 Failed Attempts

- **Fixed mask shapes (block or stripe):** caused overfitting to specific spatial patterns.  
- **Pure L2 loss:** converged fast but produced dull, over-smoothed results.  
- **Adversarial training (GAN):** required excessive tuning and gave inconsistent improvements.  

---

## 7 What I Would Do Differently

If redoing the project today, I would:
- Integrate a **ZINB prior** to better model dropout distribution.  
- Replace perceptual loss with a **contrastive embedding objective** for better  
  preservation of cell-type separability.  
- Add an **uncertainty score** for each imputed value to support downstream filtering.  

---

*Note: These reflections are based on my own development process.  
Figures used in this repository are taken from the published paper  
and are included only for academic discussion and documentation.*
