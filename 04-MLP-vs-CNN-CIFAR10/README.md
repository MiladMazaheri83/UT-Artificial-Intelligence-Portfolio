# Project 4: Deep Learning & Representation Analysis on CIFAR-10

![PyTorch](https://img.shields.io/badge/Framework-PyTorch%202.x-red)
![CUDA](https://img.shields.io/badge/Hardware-CUDA%20Accelerated-green)
![Task](https://img.shields.io/badge/Task-Image%20Classification-blue)
![Dataset](https://img.shields.io/badge/Dataset-CIFAR--10-yellow)
![Course](https://img.shields.io/badge/University%20of%20Tehran-Spring%202025-red)

This repository contains the implementation and experimental evaluation for **Assignment 4** of the Artificial Intelligence course at the **University of Tehran (Spring 2025)**. 

The primary objective is an empirical and architectural comparison between **Fully Connected Networks (MLP)** and **Convolutional Neural Networks (CNN)** under an equal parameter budget constraint (~33.5M parameters), followed by latent feature space exploration, t-SNE manifold projections, and convolutional activation map visualizations.

---

## Table of Contents

- [Overview & Benchmark Constraint](#overview--benchmark-constraint)
- [Dataset Pipeline](#dataset-pipeline)
- [Architectures & Parameter Budget](#architectures--parameter-budget)
  - [1. Deep Multi-Layer Perceptron (MLP)](#1-deep-multi-layer-perceptron-mlp)
  - [2. Deep Convolutional Neural Network (CNN)](#2-deep-convolutional-neural-network-cnn)
- [Training & Optimization Setup](#training--optimization-setup)
- [Experimental Results](#experimental-results)
- [Feature Space & Latent Representations](#feature-space--latent-representations)
  - [k-Nearest Neighbors (k-NN) Retrieval](#k-nearest-neighbors-k-nn-retrieval)
  - [t-SNE Latent Space Projection](#t-sne-latent-space-projection)
  - [Convolutional Feature Map Visualizations](#convolutional-feature-map-visualizations)
- [Project Structure](#project-structure)
- [Setup & Execution](#setup--execution)
- [License](#license)

---

## Overview & Benchmark Constraint

To conduct a fair comparative evaluation, both architectures are constrained to the same parameter budget:

$$\text{Trainable Parameters} \approx 33,500,000 \pm 500,000$$

This allows direct empirical evaluation of **inductive bias** (spatial locality, translation invariance, and weight sharing in CNNs) versus unconstrained dense connections in MLPs on natural image distributions.

---

## Dataset Pipeline

- **Dataset:** CIFAR-10 (10 object categories, $32 \times 32 \times 3$ RGB images).
- **Split:** 45,000 Training | 5,000 Validation | 10,000 Held-out Test.
- **Normalization:** Channel-wise standardization:
  - $\mu = (0.491, 0.482, 0.446)$
  - $\sigma = (0.247, 0.243, 0.261)$
- **Batch Pipeline:** Mini-batch size of $512$ with GPU pinned memory transfer.

---

## Architectures & Parameter Budget

### 1. Deep Multi-Layer Perceptron (MLP)
- **Input:** Flattened vector ($3072$ dimensions).
- **Layer Stacking:**
  - $\text{Linear}(3072 \to 5200) \to \text{ReLU} \to \text{Dropout}(0.3)$
  - $\text{Linear}(5200 \to 2600) \to \text{ReLU} \to \text{Dropout}(0.4)$
  - $\text{Linear}(2600 \to 1722) \to \text{ReLU} \to \text{Dropout}(0.5)$
  - $\text{Linear}(1722 \to 10)$
- **Parameter Distribution:** Over $92\%$ of parameters are concentrated in the first dense expansion layer ($\approx 30.98\text{M}$ parameters), lacking spatial hierarchy.

### 2. Deep Convolutional Neural Network (CNN)
- **Feature Extractor:** 3 cascaded stages of double convolutions with intermediate Max Pooling ($2\times2$, stride 2):
  - $\text{Conv}(3 \to 128) \to \text{BN} \to \text{ReLU} \to \text{Conv}(128 \to 128) \to \text{BN} \to \text{ReLU} \to \text{MaxPool}$
  - $\text{Conv}(128 \to 256) \to \text{BN} \to \text{ReLU} \to \text{Conv}(256 \to 256) \to \text{BN} \to \text{ReLU} \to \text{MaxPool}$
  - $\text{Conv}(256 \to 512) \to \text{BN} \to \text{ReLU} \to \text{Conv}(512 \to 512) \to \text{BN} \to \text{ReLU} \to \text{MaxPool}$
- **Classification Head & Latent Bottleneck:**
  - $\text{Linear}(8192 \to 3380) \to \text{ReLU} \to \text{Dropout}(0.5)$
  - $\text{Linear}(3380 \to 512)$ (**512-dim Latent Feature Space**) $\to \text{ReLU} \to \text{Dropout}(0.5)$
  - $\text{Linear}(512 \to 10)$

---

## Training & Optimization Setup

Both models were trained using identical hyperparameter regimes to ensure fair comparison:
- **Loss Function:** Categorical Cross-Entropy Loss (`nn.CrossEntropyLoss`)
- **Optimizer:** Adam ($\alpha = 10^{-4}$, weight decay $\lambda = 10^{-5}$)
- **Epoch Budget:** 30 Epochs
- **Hardware Target:** NVIDIA CUDA Acceleration

---

## Experimental Results

| Model Architecture | Trainable Parameters | Test Loss | Test Accuracy | Overfitting Onset |
| :--- | :---: | :---: | :---: | :---: |
| **Fully Connected (MLP)** | $\approx 33.48\text{M}$ | $1.7599$ | $56.09\%$ | Epoch $\sim 10$ |
| **Convolutional Network (CNN)** | $\approx 33.72\text{M}$ | **$1.1066$** | **$80.15\%$** | Epoch $\sim 6$ |

### Findings
- The **CNN outperformed the MLP by +24.06% test accuracy**, demonstrating that parameter count alone does not dictate representational power.
- Inductive biases (local receptive fields and translation invariance) prevent excessive parameter waste on positional variations, allowing the CNN to form semantically meaningful hierarchical abstractions.

---

## Feature Space & Latent Representations

The 512-dimensional bottleneck layer of the CNN was isolated to evaluate semantic clustering quality:

1. **k-Nearest Neighbors (k-NN) Retrieval:** Euclidean metric querying over the 512-D latent space verified that neighbor retrievals preserve semantic class identity even under varying backgrounds, lighting, and rotations.
2. **t-SNE Manifold Projection:** 2D dimensional reduction via t-SNE demonstrates distinct, separable topological clusters for visually distinct classes (e.g., ships vs. frogs), with predictable proximity between semantically overlapping classes (e.g., cats vs. dogs).
3. **Filter Activations:** Visualization of the initial $128$-channel convolutional feature maps confirms progressive feature detection, capturing edge orientations, color contrasts, and boundary contours.

---

## Project Structure

```text
04-MLP-vs-CNN-CIFAR10/
├── doc/
│   └── AI_S04_CA4.pdf                      # Assignment specification
├── pytorch-tutorial/
│   └── PyTorch-tutorial.ipynb              # PyTorch foundational exercises
├── notebooks/
│   └──mlp_vs_cnn_benchmark.ipynb           # Full pipeline: MLP vs CNN, t-SNE, Feature Maps
└── README.md
   ```bash
   jupyter notebook pytorch-tutorial/pytorch-tutorial.ipynb
   ```

3. **Execute Benchmark & Analysis Notebook:**
   ```bash
   jupyter notebook mlp_vs_cnn_benchmark.ipynb
   ```

---

## License

This project is part of the University of Tehran AI Course portfolio and is licensed under the [MIT License](../../LICENSE).
