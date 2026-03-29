# Detailed Analysis - Reinforcement Learning Assignment

## Executive Summary

This document provides in-depth analysis of experimental results from two reinforcement learning problems: Multi-Armed Bandits for ad campaign optimization and Q-Learning for robot navigation.

---

## Part 1: Multi-Armed Bandit Analysis

### Experimental Setup

- **Arms**: 5 ad designs
- **True CTRs**: [0.02, 0.05, 0.03, 0.08, 0.04]
- **Best arm**: Ad 3 (0.08 CTR)
- **Budget**: 10,000 impressions
- **Trials**: 50 independent runs

### Detailed Results

#### 1. Decaying ε-Greedy (Winner: 723.1 clicks)

**Why it won:**
- Started with ε=1.0 (pure exploration)
- Decayed to ε≈0.01 over ~3000 impressions
- Identified Ad 3 as best within first 1000-2000 impressions
- Spent remaining 7000-8000 impressions exploiting

**Math:**
- Early phase (2000 imp): ~50% exploration, learned ranking
- Late phase (8000 imp): ~1% exploration, 99% exploitation
- Total: 8000 × 0.08 × 0.99 + 2000 × 0.05 ≈ 732 expected clicks

#### 2. Fixed ε=0.1 (Second: 714.8 clicks)

**Performance:**
- Permanent 10% exploration throughout
- 9000 impressions to best ad: 9000 × 0.08 = 720
- 1000 random impressions: 1000 × 0.044 = 44
- Total: 764 theoretical (actual 714.8 due to learning curve)

**Why close to Decaying:**
- Identified best ad quickly (within 500-1000 impressions)
- 90% exploitation sufficient for good performance
- Robustness to drift worth the 8-click cost

#### 3. UCB Performance (Fifth: 515.9 clicks)

**Why it underperformed:**

The UCB score is: Q(a) + c × √(ln(t) / N(a))

Early in campaign (t=1000):
- Ad 3 pulled 200 times: Q(3)=0.08, bonus=√(ln(1000)/200)=0.19
- Ad 1 pulled 50 times: Q(1)=0.05, bonus=√(ln(1000)/50)=0.38

UCB selects Ad 1 despite Q(3) > Q(1) because uncertainty bonus is huge.

With large CTR gap (0.08 vs 0.05), this caution is expensive:
- Wastes impressions "confirming" weaker ads are weak
- Converges slowly to exploitation
- By time UCB commits to Ad 3, ~40-50% of budget spent suboptimally

### Contextual Variables Analysis

**Time of Day Impact:**

Fitness app (Ad 3):
- 6am CTR: 0.12 (workout planning mindset)
- 11pm CTR: 0.03 (relaxation mode)

Entertainment app (Ad 1):
- 6am CTR: 0.03
- 11pm CTR: 0.11

**Without context:**
- Average Ad 3: (0.12×0.25 + 0.03×0.25 + ...) = 0.054
- Average Ad 1: 0.071
- Agent prefers Ad 1 overall
- But serves wrong ad at wrong times

**Cost calculation:**
- 25% traffic at 6am: 2,500 impressions
- Showing Ad 1 (0.03) instead of Ad 3 (0.12)
- Lost: (0.12 - 0.03) × 2,500 = 225 clicks

---

## Part 2: Q-Learning Analysis

### Training Results

**Convergence Pattern:**
- Episodes 1-100: Avg reward -20 to +40 (random exploration)
- Episodes 100-300: Rapid improvement to ~85
- Episodes 300-1000: Fine-tuning to ~89

**Why convergence at 89 instead of theoretical 92:**
- ε=0.2 permanent exploration means 20% random actions
- Some episodes hit obstacles during random exploration
- Theoretical optimal: 8 steps × -1 + 100 = 92
- Actual: Mix of optimal paths (92) and exploration mistakes (lower)
- Average: ~89

### Hyperparameter Impact

**α (Learning Rate):**

α=0.1 (baseline):
- Q(s,a) ← Q(s,a) + 0.1 × [target - Q(s,a)]
- Smooth, stable updates
- Converges reliably

α=0.5 (high):
- Larger updates, faster initial learning
- More volatile, can overshoot
- In deterministic environment, not much benefit

**γ (Discount Factor):**

γ=0.99 (baseline):
- 8-step path: 100 × 0.99^8 = 92.27
- Can "see" full goal value from start

γ=0.5 (low):
- 8-step path: 100 × 0.5^8 = 0.39
- Goal is invisible!
- Agent can't learn purposeful navigation
- Just wanders randomly

**Mathematical proof why γ=0.5 fails:**

At position (1,0), path to goal ≈ 8 steps:
- Value = -8 (steps) + 0.39 (discounted goal) = -7.61

Agent perceives negative value for reaching goal!

### Policy Analysis

**Cell (3,3) Counterintuitive Arrow:**

Expected: RIGHT or DOWN (direct to goal)
Actual: LEFT (pointing away)

**Explanation:**
- LEFT → (3,2) → DOWN → (4,2) → RIGHT×2 → Goal = 4 steps
- Q-value ≈ -4 + 100×0.99^4 = 92

- RIGHT → (3,4) → DOWN → Goal = 2 steps (optimal)
- But during training with ε=0.2:
  - At (3,4), 20% chance of random UP
  - UP leads near obstacle (2,2) or poor positions
  - Negative experiences propagate back via Q-updates
  - Q(RIGHT at 3,3) contaminated by exploration mistakes

**Lesson:**
Fixed high exploration (ε=0.2) can cause agent to prefer safer suboptimal paths over risky optimal ones.

### Reward Sensitivity

**Current: Obstacle = -10**
- 3-step detour cost: -3
- Hit obstacle: -10
- Detour is better: -3 > -10 ✓

**Proposed: Obstacle = -0.5**
- 3-step detour cost: -3
- Hit obstacle: -0.5
- Hit obstacle is better: -0.5 > -3 ✗

Agent would bash into walls because math says it's cheaper!

**Training impact:**
- Current -10: Strong error signal, learns quickly
- Proposed -0.5: Weak error signal (20× smaller)
- Would take 5-10× more episodes to converge
- Might never learn reliable avoidance

---

## Implementation Notes

### Bandit Algorithm Complexity

**ε-Greedy:**
- Time: O(1) per action selection
- Space: O(K) for K arms (store counts and values)

**UCB:**
- Time: O(K) per action (must compute score for all arms)
- Space: O(K)

### Q-Learning Complexity

**Time:**
- Per step: O(|A|) to compute max Q(s',a')
- Per episode: O(steps × |A|)
- Total: O(episodes × steps × |A|)

**Space:**
- O(|S| × |A|) for Q-table
- In 5×5 grid: 25 states × 4 actions = 100 entries
- With defaultdict: only visited states stored

---

## Real-World Applications

### Bandit Problems

1. **Clinical Trials** - Find effective treatments while minimizing exposure to inferior ones
2. **Recommendation Systems** - Balance exploring new content vs showing proven favorites
3. **Dynamic Pricing** - Test price points while maximizing revenue

### Q-Learning Applications

1. **Robotics** - Warehouse navigation, drone control
2. **Game AI** - Learn strategies through self-play
3. **Resource Management** - Server load balancing, traffic control

---

## Conclusions

1. **No Universal Best Strategy**
   - Decaying ε-Greedy: Best for finite horizons with learning budget
   - Fixed low ε: Best for continuous deployment with drift
   - UCB: Best for small gaps, systematic exploration

2. **Discount Factor Must Match Horizon**
   - Short tasks: γ ≥ 0.95
   - Long tasks: γ can be lower
   - Deterministic tasks: γ → 1.0

3. **Reward Design Encodes Goals**
   - Must reflect real consequences
   - Can't just minimize numerical penalties
   - Test extreme cases to verify incentives

