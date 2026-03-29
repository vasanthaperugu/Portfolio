# Reinforcement Learning Fundamentals

Multi-armed bandits and Q-learning built from scratch to compare exploration strategies and learn navigation policies in a grid world.

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![NumPy](https://img.shields.io/badge/NumPy-Used-lightgrey)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange)
![Best Bandit Strategy](https://img.shields.io/badge/Best%20Bandit-Decaying%20%CE%B5--Greedy-success)
![Top Result](https://img.shields.io/badge/Best%20Clicks-723.1-brightgreen)

## Overview

This project explores two core reinforcement learning problems through simple, from-scratch implementations.

The first part uses a **multi-armed bandit** setup to model digital ad selection. The goal is to balance exploration and exploitation when the true click-through rates are unknown. Several action-selection strategies are compared over 10,000 impressions.

The second part uses **Q-learning** in a 5×5 grid world. A warehouse robot learns how to reach a goal state while avoiding obstacles. This part focuses on reward-driven learning, convergence behavior, and the effect of hyperparameters.

**Top result:** Decaying ε-Greedy achieved the strongest bandit performance with **723.1 average clicks** across 50 runs.

## Project Highlights

- Implemented reinforcement learning algorithms from scratch
- Compared multiple exploration strategies in a finite-horizon bandit problem
- Trained and evaluated a tabular Q-learning agent in a grid world
- Tested how hyperparameters changed learning behavior
- Visualized both learning curves and the final learned policy

## Methods

### Part 1: Multi-Armed Bandits
Strategies implemented:
- Random baseline
- ε-Greedy with fixed exploration
- Decaying ε-Greedy
- UCB (Upper Confidence Bound)

The bandit scenario represents a digital advertising problem with 5 ads and unknown click-through rates.

### Part 2: Q-Learning in Grid World
A tabular Q-learning agent was trained to navigate a warehouse-style 5×5 grid:
- Start state: `(0, 0)`
- Goal state: `(4, 4)`
- Obstacles: `(1, 1), (1, 3), (2, 2), (3, 1)`

Four hyperparameter settings were compared to see how learning changed under different values of alpha, gamma, and epsilon.

## Results

### Bandit Experiment
Average clicks over 50 runs:
- Random: **444.2**
- ε-Greedy (ε = 0.01): **572.6**
- ε-Greedy (ε = 0.1): **714.8**
- ε-Greedy (ε = 0.3): **680.5**
- Decaying ε-Greedy: **723.1**
- UCB (c = 2.0): **515.9**

**Takeaway:** A decaying exploration strategy performed best in this finite campaign setting. It explored early, then exploited more confidently later.

### Q-Learning Experiment
Final 100-episode averages:
- Baseline: **89.73 reward**, **10.10 steps**
- High alpha: **89.09 reward**, **10.29 steps**
- Low gamma: **89.53 reward**, **10.03 steps**
- Low epsilon: **91.56 reward**, **8.63 steps**

**Takeaway:** Lower exploration produced the best final performance in this environment. The learned policy was more direct and efficient.

## Why This Project Matters

This project shows the difference between two reinforcement learning settings:

- **Bandits** are useful when each decision is independent and feedback is immediate
- **Q-learning** is useful when decisions are connected over time and rewards depend on a full path

That contrast makes the notebook a strong portfolio piece. It shows both short-horizon optimization and sequential decision-making in one project.

## Tools

- Python
- NumPy
- Pandas
- Matplotlib
- Jupyter Notebook

## Repository Structure

```text
ReinforcementLearning_portfolio_ready.ipynb   # Main project notebook
README.md                                     # Project overview
```

## How to Run

1. Clone the repository
2. Open the notebook in Jupyter Notebook or JupyterLab
3. Run all cells in order

Install dependencies if needed:

```bash
pip install numpy pandas matplotlib jupyter
```

## Skills Demonstrated

- Reinforcement learning fundamentals
- Experimental design
- Hyperparameter analysis
- Data visualization
- Interpreting model behavior
- Writing clear technical summaries

## Future Improvements

- Add Thompson Sampling to the bandit comparison
- Compare Q-learning with SARSA
- Expand the grid world to a stochastic environment
- Add policy heatmaps or animated training visualizations
