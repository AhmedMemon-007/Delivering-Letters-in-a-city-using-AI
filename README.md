# Delivering Letters in a City — Q‑Learning Gridworld

<!-- Badges (optional). Replace `ORG/REPO` after you push to GitHub. -->

<!--
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Made with Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)]()
[![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)]()
-->

A compact, educational reinforcement learning project where an agent (the “postman”) learns, via **tabular Q‑learning**, to navigate an **11×11** city grid and deliver letters to a fixed destination.

> **Single‑file project:** `Q-LEARNING.ipynb` (Jupyter Notebook)

---

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Quick Start](#quick-start)
* [Training Configuration](#training-configuration)
* [Environment Details](#environment-details)
* [Usage Examples](#usage-examples)
* [Customize & Extend](#customize--extend)
* [FAQ](#faq)
* [License](#license)
* [Acknowledgments](#acknowledgments)

---

## Overview

* **Algorithm:** Tabular Q‑learning with ε‑greedy exploration
* **Environment:** Deterministic gridworld (size **11×11**) with passable aisles and one goal location
* **Actions:** `up`, `right`, `down`, `left` (4‑connected moves)
* **Goal:** Reach destination at **(row=0, col=5)**
* **Rewards:** `+100` (goal), `-1` (valid step), `-100` (invalid/blocked)

This repo is ideal for students and newcomers to RL who want a minimal, readable example of Q‑learning.

---

## Features

* ✅ Pure NumPy implementation (no deep learning frameworks)
* ✅ Reproducible grid setup & training loop
* ✅ Helper functions for terminal checks, action selection, transitions, and path extraction
* ✅ Greedy path rollout using the learned Q‑table
* ✅ Clear places to tweak rewards, exploration, and environment layout

---

## Quick Start

### 1) Requirements

* Python **3.9+**
* Jupyter Notebook / JupyterLab
* NumPy

```bash
pip install numpy notebook
```

### 2) Launch

```bash
jupyter notebook
```

Open **`Q-LEARNING.ipynb`** and **Run All** cells.

> Tip: For reproducibility, add `np.random.seed(42)` near the top of the notebook.

---

## Training Configuration

Default hyperparameters in the notebook:

```python
epsilon = 0.9          # ε‑greedy exploration
learning_rate = 0.9    # α
discount_factor = 0.9  # γ
n_training_episodes = 1000
```

**State space:** `(row, col)` positions on an 11×11 grid
**Action space:** 4 discrete actions
**Q‑table shape:** `(11, 11, 4)`

---

## Environment Details

The reward grid `rewards` starts at `-100` everywhere (blocked). Passable **aisles** are marked with `-1` (step cost), and the **goal** is set to `rewards[0, 5] = 100`.

**Actions (index order):**

```python
actions = ['up', 'right', 'down', 'left']
```

**Core helpers in the notebook:**

* `is_terminal_state(row, col)` — goal/blocked vs. valid step cell
* `get_starting_location()` — samples a random non‑terminal start
* `get_next_action(row, col, epsilon)` — ε‑greedy action selection
* `get_next_location(row, col, action_index)` — deterministic transition
* `get_shortest_path(start_row, start_col)` — greedy rollout from start to goal using the learned Q‑table

---

## Usage Examples

After training completes (you’ll see **“Training complete!”**):

```python
# Get a greedy path after learning
path = get_shortest_path(5, 2)
print(path)            # [[5, 2], [.., ..], ..., [0, 5]]

# More examples from the notebook
print(get_shortest_path(3, 9))
print(get_shortest_path(5, 0))
print(get_shortest_path(9, 5))
```

If the starting cell is terminal (blocked/goal), `get_shortest_path` returns `[]`.

---

## Customize & Extend

* **Grid size & aisles:** edit `environment_rows`, `environment_columns`, and the `aisles` map
* **Multiple goals:** set additional `rewards[row, col] = 100`
* **Obstacle shapes:** choose which cells are `-1` (passable) vs. `-100` (blocked)
* **Exploration schedule:** add ε‑decay (linear/exponential)
* **Visualization:** plot `np.max(q_values, axis=2)` heatmaps or overlay paths
* **Stochasticity:** inject slip/noise in `get_next_location`

<details>
<summary><strong>Optional: requirements.txt</strong></summary>

```
numpy
notebook
```

</details>

---

## FAQ

**Q: Why tabular Q‑learning instead of Deep Q‑Networks (DQN)?**
A: The state space is tiny and fully observable; tabular methods are simpler and perfectly adequate.

**Q: My agent still takes odd routes.**
A: Increase episodes, tune α/γ, or decrease ε over time (ε‑decay). Ensure your aisles and goal are configured correctly.

**Q: How do I render the path?**
A: Add a simple matplotlib plot to draw the grid and connect consecutive coordinates in `path`.

---

## License

Add your preferred license (e.g., **MIT**) to the repository root.

---

## Acknowledgments

Inspired by classic gridworld Q‑learning examples (see Sutton & Barto, *Reinforcement Learning: An Introduction*).
