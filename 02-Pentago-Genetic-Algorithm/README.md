# Project 2: Evolutionary Optimization & Adversarial Game Search

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Genetic Algorithm](https://img.shields.io/badge/Optimization-Genetic%20Algorithm-purple)
![Game Search](https://img.shields.io/badge/Adversarial-Minimax%20%26%20Alpha--Beta-orange)
![Course](https://img.shields.io/badge/University%20of%20Tehran-Spring%202025-red)

This repository contains implementations for **Assignment 2** of the Artificial Intelligence course at the **University of Tehran (Spring 2025)**. The project is split into two major components:
1. **Genetic Algorithm (GA):** Continuous parameter optimization to approximate Fourier series coefficients of arbitrary target functions.
2. **Adversarial Search (Pentago):** An intelligent agent playing the 6x6 board game **Pentago** using Minimax with Alpha-Beta pruning and dynamic board evaluation heuristics.

---

## Table of Contents

- [Project 2: Evolutionary Optimization \& Adversarial Game Search](#project-2-evolutionary-optimization--adversarial-game-search)
  - [Table of Contents](#table-of-contents)
  - [Part 1: Fourier Series Approximation (Genetic Algorithm)](#part-1-fourier-series-approximation-genetic-algorithm)
    - [Problem Formulation](#problem-formulation)
    - [Genetic Operators](#genetic-operators)
    - [Fitness Metrics](#fitness-metrics)
  - [Part 2: Pentago Game AI (Adversarial Search)](#part-2-pentago-game-ai-adversarial-search)
    - [Game Mechanics](#game-mechanics)
    - [Minimax with Alpha-Beta Pruning](#minimax-with-alpha-beta-pruning)
    - [Heuristic Evaluation Function](#heuristic-evaluation-function)
  - [Project Structure](#project-structure)
  - [Setup \& Execution](#setup--execution)
  - [License](#license)

---

## Part 1: Fourier Series Approximation (Genetic Algorithm)

### Problem Formulation

Given an unknown continuous target function $f(x)$ over an interval, the objective is to approximate its trajectory using a truncated Fourier series expansion:

```text
f_approx(x) = a0/2 + Σ [ a_n * cos(n * x) + b_n * sin(n * x) ]   for n = 1 to N
```

- **Chromosome Representation:** A real-valued vector consisting of $2N + 1$ genes:
  ```text
  Chromosome = [ a0, a1, a2, ..., aN, b1, b2, ..., bN ]
  ```
- **Search Space:** Continuous domain where each coefficient is bounded within $[-10, 10]$.

### Genetic Operators

- **Selection Strategies:**
  - *Tournament Selection:* Selects best individual among randomly sampled contenders.
  - *Roulette Wheel Selection:* Fitness-proportional probability distribution.
  - *Rank-Based Selection:* Ranks individuals to mitigate premature convergence.
- **Crossover Operators:**
  - Single-point, Multi-point, and Uniform Crossover for real-valued chromosomes.
- **Mutation:**
  - Random Gaussian/Uniform perturbation applied to selected genes followed by clamping within valid intervals.

### Fitness Metrics

Fitness is computed over uniformly sampled points using inverse loss metrics:
- **Root Mean Squared Error (RMSE):** Reciprocal loss for high precision.
- **Mean Absolute Error (MAE):** Robust against outlier penalty spikes.
- **Huber Loss:** Smooth blend between linear and quadratic penalties.

---

## Part 2: Pentago Game AI (Adversarial Search)

### Game Mechanics

Pentago is a two-player zero-sum game played on a 6x6 grid composed of four 3x3 rotatable quadrants:
1. **Move Phase:** Place a marble on any empty grid cell.
2. **Rotation Phase:** Rotate one of the four 3x3 sub-grids 90 degrees clockwise or counterclockwise.
3. **Winning Condition:** Align 5 consecutive marbles horizontally, vertically, or diagonally.

### Minimax with Alpha-Beta Pruning

- **Minimax Tree Search:** Explores the game tree where the agent maximizes expected payoff while assuming optimal adversarial play.
- **Alpha-Beta Pruning:** Maintains bounds `[alpha, beta]` to prune unpromising subtrees without loss of optimality.
- **Branching Factor Handling:** Sub-grid rotations exponentially expand the branching factor ($b \approx \text{empty-cells} \times 8$). Depth-limited search is paired with an evaluation function to satisfy strict time constraints.

### Heuristic Evaluation Function

For non-terminal leaf nodes at maximum search depth:
- Evaluates unblocked 5-cell sequences across rows, columns, and diagonals.
- Assigns exponential weights based on the density of friendly vs. opponent pieces in potential winning lines.
- Assigns asymptotic high/low scores to immediate terminal win/loss configurations.

---

## Project Structure

```text
02-Pentago-Genetic-Algorithm/
├── doc/
|   ├── handwritten-report.pdf/ # Assignment report
│   └── AI-S04-CA2.pdf              # Assignment specification
├── fourier-ga/
│   └── Genetic-Algorithm.ipynb     # Fourier approximation & GA operator experiments
├── pentago-minimax/
│   └── Minmax-Algorithm.ipynb      # Pentago environment, Minimax, and Alpha-Beta agent
└── README.md
```

---

## Setup & Execution

1. **Install Dependencies:**
   ```bash
   pip install numpy scipy matplotlib pygame
   ```

2. **Run Fourier GA Experiments:**
   ```bash
   jupyter notebook fourier-ga/Genetic-Algorithm.ipynb
   ```

3. **Run Pentago AI & Benchmarks:**
   ```bash
   jupyter notebook pentago-minimax/Minmax-Algorithm.ipynb
   ```

---

## License

This project is part of the University of Tehran AI Course portfolio and is licensed under the [MIT License](../LICENSE).
