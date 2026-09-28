# Project 1: Portal-Aware Puzzle Solver (Search Algorithms)

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Search Algorithms](https://img.shields.io/badge/AI-Classic%20Search-orange)
![Course](https://img.shields.io/badge/University%20of%20Tehran-Spring%202025-red)

This project implements and evaluates various **uninformed** and **informed** classical search algorithms to solve a grid-based puzzle environment. The environment is an extended Sokoban-style game featuring dynamic teleportation portals where the agent must push numbered boxes into their corresponding targets with minimal cost.

---

## Table of Contents

- [Problem Formulation](#problem-formulation)
- [Environment & Rules](#environment--rules)
- [Algorithms Implemented](#algorithms-implemented)
- [Heuristic Design](#heuristic-design)
- [Project Structure](#project-structure)
- [Execution & Usage](#execution--usage)
- [Algorithm Comparison Summary](#algorithm-comparison-summary)

---

## Problem Formulation

- **State Representation:** 
  - Player's grid coordinates $(x, y)$
  - Positions of all numbered boxes $\{(x_1, y_1), (x_2, y_2), \dots, (x_k, y_k)\}$
  - Portal pairing configurations
- **Action Space:** 
  - $\mathcal{A} \in \{\text{Up}, \text{Down}, \text{Left}, \text{Right}\}$
- **Transition Model:** 
  - Validates boundary checks, wall collisions, pushing single boxes into open spaces, and directional portal teleportation.
- **Goal Test:** 
  - All numbered boxes occupy their uniquely designated goal tiles: $\forall i \in \{1, \dots, k\}, \text{Box}_i = \text{Goal}_i$.
- **Path Cost:** 
  - Step cost is uniform ($c(s, a, s') = 1$ per move).

---

## Environment & Rules

1. **Agent Movement:** The agent navigates open cells and pushes adjacent boxes if the space behind the box is unobstructed.
2. **Portals:** Entering a portal instantly teleports the entity (agent or box) to the paired portal's coordinates, preserving the direction of entry. Up to two distinct portal pairs are supported.
3. **Deadlock Detection:** Terminal states where boxes are pushed into non-goal corners or irreversible configurations are pruned to optimize the state space.

---

## Algorithms Implemented

### 1. Uninformed Search
- **Breadth-First Search (BFS):** Guarantees the optimal (shortest path) solution by exploring the state space level-by-level using a FIFO queue.
- **Depth-First Search (DFS):** Memory-efficient exploration using a LIFO stack; finds non-optimal solutions and is susceptible to deep branch traversal.
- **Iterative Deepening Search (IDS):** Combines the space efficiency of DFS with the completeness and optimality guarantees of BFS by incrementally increasing depth limits.

### 2. Informed Search
- **A\* Search:** Utilizes path cost $g(n)$ and heuristic estimate $h(n)$ with an evaluation function $f(n) = g(n) + h(n)$. Guaranteed to find optimal solutions when $h(n)$ is admissible and consistent.
- **Weighted A\* Search:** Introduces an inflation parameter $w \ge 1$ to trade optimality for search speed:
  $$f(n) = g(n) + w \cdot h(n)$$

---

## Heuristic Design

To accelerate the search space exploration in A* and Weighted A*, multiple heuristic variants were designed:

1. **Standard Manhattan Distance (h1):**
   Sum of Manhattan distances between each box and its matching target:
   ```text
   h1(n) = Σ |Box_i.x - Goal_i.x| + |Box_i.y - Goal_i.y|
   ```

2. **Portal-Aware Manhattan Distance (h2):**
   Calculates the minimum path cost by comparing direct traversal against shortcut routes passing through available portal pairs (P1, P2):
   ```text
   D_portal(A, B) = min(
       Manhattan(A, B),
       min_{(P1, P2)} [ Manhattan(A, P1) + Manhattan(P2, B) + 1 ]
   )

   h2(n) = Σ D_portal(Box_i, Goal_i)
   ```

3. **Composite Admissible Heuristic (h3):**
   Incorporates the minimum distance from the player to unplaced boxes to account for agent positioning cost:
   ```text
   h3(n) = Σ D_portal(Box_i, Goal_i) + min_{i ∈ unplaced} D_portal(Agent, Box_i)
   ```

> **Note:** Admissibility (h(n) ≤ h*(n)) and consistency (h(n) ≤ c(n, a, n') + h(n')) guarantee an optimal solution when w = 1.

---

## Project Structure
```text
01-Search-Game-Solver/
├── doc/
|   ├── handwritten-report.pdf/ # Assignment report
│   └── AI-S04-CA1.pdf          # Assignment specification
├── src/
│   ├── assets/
│   │   ├── maps/               # Test map configurations
│   │   ├── sounds/             # Game sound effects
│   │   └── sprites/            # Game visual assets
│   ├── game.py                 # Core game engine & state representations
│   ├── gui.py                  # Graphical interface & visualization
│   ├── h.ipynb                 # Heuristic experiments & analysis
│   ├── notebook.ipynb          # Solver implementations (BFS, DFS, IDS, A*, Weighted A*)
│   └── README.md
└── README.md
```

---

## Execution & Usage

1. **Install Dependencies:**
   ```bash
   pip install numpy
   ```

2. **Run Notebook:**
   Open and execute `notebook.ipynb` in your preferred Jupyter environment to run individual test cases and benchmark solvers across maps:
   ```bash
   jupyter notebook notebook.ipynb
   ```

---

## Algorithm Comparison Summary

| Metric | BFS | DFS | IDS | A\* | Weighted A\* ($w > 1$) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Completeness** | Yes | Yes (finite) | Yes | Yes | Yes |
| **Optimality** | Yes | No | Yes | Yes (with admissible $h$) | $\epsilon$-suboptimal ($\le w \cdot C^*$) |
| **Time Complexity** | $\mathcal{O}(b^d)$ | $\mathcal{O}(b^m)$ | $\mathcal{O}(b^d)$ | $\mathcal{O}(b^{\epsilon d})$ | Significantly Faster than A\* |
| **Space Complexity** | $\mathcal{O}(b^d)$ | $\mathcal{O}(bm)$ | $\mathcal{O}(bd)$ | $\mathcal{O}(b^d)$ | Reduced frontier size |

---

## License

This project is part of the University of Tehran AI Course portfolio and is licensed under the [MIT License](../../LICENSE).
```
