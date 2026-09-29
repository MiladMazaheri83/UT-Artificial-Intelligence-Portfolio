# Project 3: Student Academic Performance Classification

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Machine Learning](https://img.shields.io/badge/ML-Supervised%20Classification-green)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![XGBoost](https://img.shields.io/badge/Library-XGBoost-blueviolet)
![Course](https://img.shields.io/badge/University%20of%20Tehran-Spring%202025-red)

This project constitutes **Assignment 3** of the Artificial Intelligence course at the **University of Tehran (Spring 2025)**. The objective is to formulate and solve a supervised multi-class classification task predicting students' final performance in an Artificial Intelligence course using demographic, social, behavioral, and academic predictors.

---

## Table of Contents

- [Project 3: Student Academic Performance Classification](#project-3-student-academic-performance-classification)
  - [Table of Contents](#table-of-contents)
  - [Problem Definition](#problem-definition)
  - [Dataset \& Feature Pipeline](#dataset--feature-pipeline)
    - [Feature Engineering \& Cleaning](#feature-engineering--cleaning)
    - [Target Discretization](#target-discretization)
    - [Data Splitting \& Scaling](#data-splitting--scaling)
  - [Models Implemented](#models-implemented)
    - [Custom Decision Tree (From Scratch)](#custom-decision-tree-from-scratch)
    - [Ensemble \& Baseline Models](#ensemble--baseline-models)
  - [Evaluation \& Benchmark Results](#evaluation--benchmark-results)
  - [Project Structure](#project-structure)
  - [Setup \& Execution](#setup--execution)
  - [License](#license)

---

## Problem Definition

Predicting student academic outcome enables early intervention for students at risk of academic failure. The continuous final course grade (`finalGrade` $\in [0, 20]$) is cast into an **ordered 4-class classification problem**:

| Class | Performance Level | Final Grade Range | Categorical Label |
| :---: | :---: | :---: | :---: |
| **0** | Excellent (A) | $\ge 17$ | Highly Successful |
| **1** | Good (B) | $[14, 17)$ | Above Average |
| **2** | Average (C) | $[10, 14)$ | Pass |
| **3** | Failing (D) | $< 10$ | Fail / Academic Warning |

---

## Dataset & Feature Pipeline

### Feature Engineering & Cleaning
- **Input Dimensions:** 395 student instances loaded from `data/Grades.csv`.
- **Feature Pruning:** Domain-irrelevant attributes (`fatherJob`, `motherJob`, `reason`) with negligible correlation to student academic yield were eliminated.
- **Categorical Encoding:** Binary features (`address`, `higher`, `internet`, `romantic`, etc.) mapped to binary numeric flags (`0/1`); university identifier mapped as `PR=1` (Princeton), `CM=0` (Carnegie Mellon).

### Target Discretization
The continuous target `finalGrade` is mapped into integer category representations via a deterministic partition mapping `grade_to_category()`.

### Data Splitting & Scaling
- **Stratified Partitioning:** Stratified splitting applied to guarantee identical class distributions across training, validation, and testing subsets (Train: $80\%$, Test: $20\%$ with an isolated held-out partition).
- **Leak-Free Normalization:** Scalers (`StandardScaler`, `MinMaxScaler`, `RobustScaler`) fitted strictly on training distributions and applied out-of-sample to validation and held-out subsets to prevent data leakage.

---

## Models Implemented

### Custom Decision Tree (From Scratch)
A recursive binary decision tree classifier implemented directly via NumPy:
- **Splitting Criterion:** Shannon Entropy reduction (Information Gain):
  $$H(S) = - \sum_{c=1}^{C} p_c \log_2(p_c)$$
  $$IG(S, A) = H(S) - \sum_{v \in Values(A)} \frac{|S_v|}{|S|} H(S_v)$$
- **Stopping Conditions:** Maximum tree depth threshold (`max_depth`) or node purity ($H(S) = 0$).
- **Inference:** Recursive node traversal with majority voting fallback at leaves.

### Ensemble & Baseline Models
1. **Gaussian Naive Bayes (`GaussianNB`):** Generative baseline modeling continuous features under normal density assumptions.
2. **Scikit-Learn Decision Tree (`DecisionTreeClassifier`):** Optimized CART baseline.
3. **Random Forest (`RandomForestClassifier`):** Bagging ensemble of de-correlated decision trees mitigating variance.
4. **XGBoost (`XGBClassifier`):** Gradient-boosted decision trees optimizing multi-class logarithmic loss.

---

## Evaluation & Benchmark Results

Performance was evaluated on held-out test splits using Accuracy, Precision, Recall, and Macro/Weighted $F_1$-score.

| Model | Test Accuracy | Macro $F_1$ | Weighted $F_1$ | Status |
| :--- | :---: | :---: | :---: | :---: |
| **Gaussian Naive Bayes** | $66.18\%$ | $0.64$ | $0.66$ | Scikit-Learn |
| **Decision Tree (From Scratch)** | $83.82\%$ | $0.83$ | $0.83$ | Custom NumPy |
| **Random Forest Classifier** | $85.29\%$ | $0.85$ | $0.85$ | Scikit-Learn |
| **Decision Tree (Scikit-Learn)** | **$86.76\%$** | **$0.86$** | **$0.86$** | Scikit-Learn |
| **XGBoost Classifier** | Evaluated | — | — | DMLC XGBoost |

*Note: The Random Forest ensemble exhibited optimal generalization balance, yielding $83.33\%$ accuracy and $0.84$ Macro $F_1$ on the unseen held-out validation slice.*

---

## Project Structure

```text
03-Student-Performance/
├── data/
│   └── Grades.csv              # Student academic & demographic dataset
├── doc/
│   └── AI-S04-CA3.pdf          # Assignment specification
├── notebooks/
│   └──DataAnalyse.ipynb        # EDA, preprocessing, scratch & ensemble models
└── README.md
```

---

## Setup & Execution

1. **Install Required Packages:**
   ```bash
   pip install numpy pandas scikit-learn xgboost matplotlib seaborn
   ```

2. **Run Analysis Notebook:**
   ```bash
   jupyter notebook DataAnalyse.ipynb
   ```

---

## License

This project is part of the University of Tehran AI Course portfolio and is licensed under the [MIT License](../../LICENSE).
