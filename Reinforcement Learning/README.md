# Reinforcement Learning Fundamentals
## Multi-Armed Bandits and Q-Learning from Scratch

This project explores two core reinforcement learning ideas through clean Python implementations and interpretable experiments:

- **Multi-Armed Bandits** for exploration vs. exploitation
- **Q-Learning** for sequential decision-making in a grid world

The goal was to move beyond theory and show how reinforcement learning behaves in practical settings, with clear experiments, visualizations, and takeaways.

## Project Overview

This notebook is split into two parts.

### 1) Multi-Armed Bandits: Digital Ad Optimization
A simulated advertising setting is used to compare action-selection strategies when the true click-through rates are unknown. Each impression creates a tradeoff between:

- **exploration**: trying different ads to learn more
- **exploitation**: favoring the ad that currently looks best

Strategies implemented from scratch:
- Random baseline
- ε-Greedy
- Decaying ε-Greedy
- UCB (Upper Confidence Bound)

### 2) Q-Learning: Warehouse Grid Navigation
A tabular Q-learning agent learns to move through a 5×5 warehouse grid from start to goal while avoiding blocked cells. This part focuses on:

- learning from reward and repeated interaction
- how hyperparameters affect training behavior
- how a learned policy can be inspected visually, not just numerically

## Why This Project Matters

This notebook shows two different reinforcement learning problem types:

- **Bandits** are useful when decisions are immediate and feedback comes quickly.
- **Q-learning** is useful when decisions are connected across time and early choices affect later outcomes.

That distinction matters in real applications such as ad ranking, recommendations, robotics, navigation, and resource allocation.

## Key Results

### Bandit Experiment
- **Decaying ε-Greedy** performed best over a fixed 10,000-impression campaign.
- Early exploration helped identify the strongest ad.
- Continued random exploration became expensive later in the run.
- **UCB** was strong, but did not beat the best ε-based strategy in this setup.

### Q-Learning Experiment
- The agent improved quickly during early training and then stabilized.
- A lower exploration rate produced the best final performance in the tested configurations.
- Hyperparameters changed both convergence speed and policy quality.
- Visualizing the learned policy helped confirm that the agent found sensible routes around obstacles.

## What’s Included

- reinforcement learning algorithms implemented from scratch
- experiment design for comparing strategies
- performance plots and policy visualization
- interpretation of results in plain language
- reflection on practical tradeoffs and next steps

## Tools Used

- Python
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Jupyter Notebook

## Repository Structure

```text
ReinforcementLearning_portfolio_ready.ipynb   # Main project notebook
README.md                                     # Project summary
```

## How to Run

1. Clone the repository.
2. Open the notebook in Jupyter Notebook or JupyterLab.
3. Run all cells in order.

Install dependencies if needed:

```bash
pip install numpy pandas matplotlib seaborn jupyter
```

## What I Learned

A few things stood out while building this project:

- exploration has a real cost in finite-horizon problems
- hyperparameters shape learning behavior more than they may seem at first
- good plots matter because they make it easier to see whether the model is learning something useful
- reinforcement learning is easier to understand when results are tied to realistic scenarios instead of only formulas

## Possible Next Steps

- add **Thompson Sampling** to the bandit comparison
- compare **Q-Learning** with **SARSA**
- extend the navigation task to a larger or stochastic environment
- experiment with a function-approximation or deep RL approach

## Portfolio Value

This project highlights:

- algorithm implementation from scratch
- experiment design and comparison
- result interpretation
- visualization skills
- the ability to connect technical work to practical decision-making problems

