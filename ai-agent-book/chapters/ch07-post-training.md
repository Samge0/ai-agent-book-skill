# Chapter 7: Model Post-Training

## Core Idea
SFT memorizes, RL generalizes. The industry recipe: **lay the foundation with SFT, then climb with RL**. SFT establishes "form" `format, structure`; RL pursues "spirit" `strategy, generalization`. The real differentiator is not the algorithm but data quality and environment fidelity.

## Frameworks Introduced

- **Three-Stage Pipeline**: Pre-training, then SFT, then RL
  - When to use: Any model post-training
  - How: Pre-training provides knowledge; SFT provides format; RL provides strategy

- **SFT vs RL** `the most important table`:
  - SFT: Optimizes "how much it looks like the standard answer" -> memorizes fixed mappings
  - RL: Optimizes "how good the result is" -> learns transferable strategies
  - When to use: Choosing training method
  - How: SFT for format/style/process + high-quality demonstrations; RL for generalization + exploring optimal strategies

- **Mode-seeking vs Mass-covering**:
  - SFT = mass-covering `spreads probability across all modes in data`
  - RL = mode-seeking `concentrates probability on highest-reward modes`
  - When to use: Understanding why RL models are more decisive and focused
  - How: Reverse KL divergence drives mode-seeking; forward KL drives mass-covering

- **Verification-Generation Asymmetry**: Verifying is easier than generating
  - When to use: Deciding if RL can climb higher than SFT
  - How: Math answers checked against key; code run through tests; theorem provers verify proofs. As long as recognizing good work is easier than producing it, RL can train stronger than any demonstrator.

- **RLHF** `Reinforcement Learning from Human Preferences`: From human preferences to reward models
  - When to use: When outcome rewards are hard to define automatically
  - How: Collect preference pairs, train reward model, use RM for RL training

- **Process vs Outcome Rewards**:
  - **Outcome reward**: Single signal at the end — sparse but easy to define
  - **Process reward**: Step-by-step feedback — dense but expensive to obtain
  - When to use: Multi-turn tasks with long trajectories
  - How: "Reward the outcome, constrain the process" — RLVP and partial rewards

- **PPO vs GRPO Comparison**:
  - **PPO**: Uses value network for finer-grained credit assignment; stable but expensive
  - **GRPO**: Samples N trajectories, compares within group; no value network needed; cheaper but coarser credit
  - When to use: Algorithm selection
  - How: GRPO for single-turn/short-trajectory; PPO for multi-turn/long-trajectory

## Key Concepts
- **On-Policy vs Off-Policy**: On-policy uses only current policy data; off-policy uses data from other/older policies
- **Covariate Shift**: Offline imitation error grows as T-squared with trajectory length; online compresses to T
- **Policy Gradient**: Adjust parameters in direction that increases expected return
- **Advantage**: How much better an action is than average `reduces variance`
- **DPO**: Offline preference optimization; directly turns preference pairs into classification loss
- **Best-of-N**: Generate N outputs, select best; inference-time method, no training
- **Reward Hacking**: Agent finds shortcut to high scores without actually completing the task
- **RLVR**: Reinforcement Learning from Verifiable Rewards — rule-based verification instead of learned RM

## Mental Models
- **Think of SFT as tracing a map**: At best it is as good as the map; ceiling = demonstrator quality
- **Think of RL as exploring with a compass**: Can walk off the map; ceiling = the task itself
- **Think of mode-seeking as winner-takes-all**: RL concentrates on few good strategies; SFT spreads probability
- **Use SFT when you have good demonstrations, RL when you have good rewards**

## Anti-patterns
- **Doing RL before SFT on a weak base model**: Without stable output format, reward signal is just noise
- **Optimizing the algorithm before fixing data/environment**: Prior: base model > environment > algorithm
- **Using outcome rewards for long multi-turn tasks**: Sparse signal causes credit assignment problems
- **Mass-covering when you need decisiveness**: SFT spreads probability; RL concentrates on best strategies

## Code Examples
```python
# GRPO advantage estimation: no value network needed
# For the same question, sample N trajectories

# N=4 trajectories with rewards: r = [0.8, 0.2, 0.6, 0.4]
import numpy as np
r = [0.8, 0.2, 0.6, 0.4]
mean_r = np.mean(r)  # 0.5
std_r = np.std(r)    # 0.2236

# Advantage: "positive if better than group average, negative if worse"
advantages = [(ri - mean_r) / std_r for ri in r]
# [1.118, -1.342, 0.447, -0.224]
# Trajectory 0 reinforced most; trajectory 1 penalized most
```
- **What it demonstrates**: GRPO eliminates the value network by using intra-group relative comparison. Each trajectory advantage is its relative performance within the group — "positive if better than average, negative if worse."

## Reference Tables

### SFT vs RL Essential Comparison
| Dimension | SFT | RL |
|-----------|-----|-----|
| Optimization Objective | Maximize probability of labeled answer | Maximize expected reward |
| Training Signal | Single standard answer | Multiple self-generated responses + reward |
| What is Learned | Fixed mapping `memorization` | Transferable strategy `generalization` |
| Under Distribution Shift | Applies old answer, performance drops | Re-solves using same strategy |
| Sample Efficiency | High `thousands of examples` | Low `tens to hundreds of times SFT` |
| Ceiling | Demonstrator quality | The task itself |
| Best Suited For | Format/style, demonstrations, stable env | Generalization, exploring strategies |

### Algorithm Selection Guide
| Method | Type | Core Idea | Best For |
|--------|------|-----------|----------|
| **PPO** | Online RL | Clip to limit update; value network | Multi-turn, long trajectories |
| **GRPO** | Online RL | Group-relative comparison; no value net | Single-turn, short trajectories |
| **DPO** | Offline pref opt | Preference pairs to classification loss | High-quality preference data |
| **Best-of-N** | Inference-time | Generate N, select best | Early exploration, quick start |

## Worked Example
**GeneralPoints card game `Experiment 7-11`**: In training, J/Q/K are all worth 10. SFT memorizes "J/Q/K means 10." At test time, J becomes 11. SFT still plays 10 — gets it wrong. RL learned the strategy "what process leads to correct result" — when J becomes 11, it recalculates using the same strategy and gets it right. This is the essence: SFT memorizes mappings; RL generalizes strategies.

## Key Takeaways
1. **SFT first, then RL**: SFT establishes "form"; RL pursues "spirit" — form first, spirit second.
2. **The ceiling of SFT is the data; the ceiling of RL is the task**: RL can discover strategies not in demonstrations.
3. **Verification-generation asymmetry powers RLVR**: When checking answers is easier than producing them, RL climbs higher.
4. **Prior: base model > environment > algorithm**: Do not get hung up on algorithms; fix data and environment first.
5. **Use GRPO for cost efficiency, PPO for long-trajectory credit assignment**: The difference is the advantage estimation method.
6. **Reward the outcome, constrain the process**: Multi-turn tasks need dense signals but cannot sacrifice outcome quality.
7. **Online methods recover from their own mistakes**: Offline imitation suffers covariate shift; online trains on the path it will walk.

## Connects To
- **Ch 1**: Three learning paradigms — post-training is the highest-cost, permanent option
- **Ch 6**: Evaluation environments and reward signals drive training
- **Ch 8**: Externalized learning as alternative to post-training when cost is prohibitive
- **Ch 9**: VLA robotics training — imitation learning + RL; Sim2Real transfer
- **Ch 10**: Multi-agent systems consuming RL-trained models; RLEF `execution feedback`
