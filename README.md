# ⚡ Stochastic Gradient Descent Sampling Schemes — Experimental Analysis

This repository contains a **Jupyter Notebook** that experimentally analyzes the behavior of **Stochastic Gradient Descent (SGD)** under two distinct data sampling schemes:

1.  **SGD with replacement** (The **theoretical** approach often used in formal analysis).
2.  **SGD without replacement** (The **heuristic** approach commonly used in **practice**).

The primary goal is to investigate whether the increased computational cost of *SGD with replacement* translates into any practical **performance advantage** over the standard, more memory-efficient *SGD without replacement*.

---

## 🔬 Experiment Overview

### **Dataset and Model**

* **Dataset:** [Iris Dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#iris-plants-dataset)
    * *Details:* 150 samples, 4 features, 3 classes.
* **Model Architecture:** A simple, shallow **PyTorch** network. 
    ```python
    model = nn.Sequential(
        nn.Linear(4, 4),
        nn.ReLU(),
        nn.Linear(4, 3),
    )
    ```

### **Training Configuration**

| Parameter | Value | Note |
| :--- | :--- | :--- |
| **Optimizer** | SGD | Learning Rate = 0.01 |
| **Batch Size** | 1 | Used to highlight **pure SGD behavior** |
| **Epochs** | 100 | |
| **Framework** | PyTorch | |
| **Runs** | 30 | Independent runs per scheme for statistical evaluation |

### **Methodology**

1.  **Data Preparation:** Iris dataset loaded directly via `scikit-learn`.
2.  **Mini-Batch Construction:** Custom batching logic implemented to rigorously control the **replacement vs. non-replacement** sampling toggle.
3.  **Metrics:** Train/Test accuracy and loss recorded for every epoch of every run.
4.  **Evaluation:** **95% Confidence Intervals** computed across the 30 independent runs for each scheme.

---

## 📈 Key Experimental Findings

### **Qualitative Behavior Comparison**

The table below summarizes the core observed differences in training dynamics. 

| Scheme | Gradient Norm Behavior | Early Convergence | Variance of Metrics |
| :--- | :--- | :--- | :--- |
| **With Replacement** | Larger range, **higher variance** | Reaches near-100% test accuracy **faster** | Higher |
| **Without Replacement** | Smaller range, **more stable** | Slightly **slower** initial convergence | Lower |

> Both schemes ultimately achieved **similar average performance** across all 30 runs.

### **95% Confidence Interval Results (Final Epoch)**

The following table presents the final **95% confidence intervals** for the recorded metrics.

| Metric | With Replacement | Without Replacement |
| :--- | :--- | :--- |
| **Train Accuracy** | $(91.07\%, 93.07\%)$ | $(91.22\%, 93.10\%)$ |
| **Test Accuracy** | $(90.60\%, 92.69\%)$ | $(90.37\%, 93.60\%)$ |
| **Train Loss** | $(0.1862, 0.2336)$ | $(0.1799, 0.2261)$ |
| **Test Loss** | $(0.1869, 0.2341)$ | $(0.1667, 0.2227)$ |

---

## 📝 Conclusion

The confidence intervals show **no statistically significant performance advantage** for either sampling scheme in the long run.

| Observation | Interpretation |
| :--- | :--- |
| **With-replacement** SGD converged **faster early on** | The higher stochasticity may increase **exploration** in the initial training phase. |
| **Without-replacement** SGD was **more stable** | Reduced variance in gradient norms and recorded losses. |
| **Long-run performance similar** | Confirms the theoretical equivalence in expectation of the two schemes. |

> **Overall Verdict:** Adopting SGD with replacement provides **no significant practical gain** while consuming substantially **more memory** (for the reservoir sampling required). **SGD without replacement** remains the more memory-efficient and equally effective option for real-world application.

---

## 🛠 Reproduction and Reference

Dependencies are listed in `requirements.txt`.
>To run:
```bash
pip install -r requirements.txt
jupyter notebook SGD_Sampling_Behavior.ipynb
```
---
*Author: Irene Xu*

*Date: November 2025*