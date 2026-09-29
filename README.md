# UT-Artificial-Intelligence-portfolio

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Course](https://img.shields.io/badge/University%20of%20Tehran-Spring%202025-red)
![License](https://img.shields.io/badge/License-MIT-green)

This repository contains the assignments and implementations for the **Artificial Intelligence** course at the **University of Tehran (Spring 2025)**. The projects cover search strategies, adversarial games, evolutionary computation, classical machine learning, deep learning, and unsupervised clustering.

---

## Table of Contents

- [UT-Artificial-Intelligence-portfolio](#ut-artificial-intelligence-portfolio)
  - [Table of Contents](#table-of-contents)
  - [Projects Overview](#projects-overview)
    - [Project 1: Search Algorithms (Game Solver)](#project-1-search-algorithms-game-solver)
    - [Project 2: Adversarial Search \& Optimization](#project-2-adversarial-search--optimization)
    - [Project 3: Supervised Classification](#project-3-supervised-classification)
    - [Project 4: Deep Learning for Computer Vision](#project-4-deep-learning-for-computer-vision)
  - [Repository Structure](#repository-structure)
  - [Setup \& Requirements](#setup--requirements)
  - [License](#license)

---

## Projects Overview

### Project 1: Search Algorithms (Game Solver)
Implemented a grid-based puzzle solver where the agent must navigate boxes to designated targets utilizing dynamic portal mechanics.
- **Uninformed Search:** Breadth-First Search (BFS), Depth-First Search (DFS), Iterative Deepening Search (IDS).
- **Informed Search:** A* and Weighted A* using admissible and consistent heuristic designs to minimize expanded states and total path cost.

### Project 2: Adversarial Search & Optimization
- **Part 1 (Genetic Algorithm):** Approximated the continuous Fourier series coefficients of an arbitrary target function via selection, crossover, and mutation operators.
- **Part 2 (Adversarial Search):** Built a game agent for the **Pentago** board game using Minimax with **Alpha-Beta Pruning** and custom heuristic evaluation functions for state evaluation.

### Project 3: Supervised Classification
Predicting academic outcomes (final course grades) based on demographic, behavioral, and academic performance metrics.
- **Data Pipeline:** Feature engineering, categorical encoding, missing value imputation, and class imbalance mitigation.
- **Models:** Gaussian Naive Bayes, Decision Trees, Random Forests, and XGBoost.
- **Evaluation:** Evaluated using precision, recall, weighted F1-score, and ROC-AUC curves.

### Project 4: Deep Learning for Computer Vision
A comparative empirical study of feed-forward versus convolutional architectures on the **CIFAR-10** benchmark.
- **Architectures:** Multi-Layer Perceptron (MLP) baseline vs. Deep Convolutional Neural Network (CNN).
- **Techniques:** Dropout, Batch Normalization, learning rate schedules, and data augmentation.
- **Analysis:** Loss/accuracy trajectories, confusion matrix analysis, and intermediate layer feature map visualizations.

---

## Repository Structure

```text
├── 01-Search-Game-Solver/
|   ├── doc/
│   ├── src/
│   └── README.md
├── 02-Pentago-Genetic-Algorithm/
|   ├── doc/
│   ├── fourier-ga/
│   ├── pentago-minimax/
│   └── README.md
├── 03-Student-Performance/
│   ├── data/
|   ├── doc/
│   ├── notebooks/
│   └── README.md
├── 04-MLP-vs-CNN-CIFAR10/
│   ├── doc/
│   ├── notebooks/
│   ├── pytorch-tutorial/
│   └── README.md
├── README.md
└── requirements.txt
```

---

## Setup & Requirements

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```

2. **Environment Setup:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Core Dependencies:**
   - `numpy`, `scipy`, `pandas`
   - `scikit-learn`, `xgboost`
   - `torch`, `torchvision`
   - `sentence-transformers`
   - `matplotlib`, `seaborn`

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.