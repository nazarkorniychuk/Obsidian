---
type: moc
topics: [reinforcement-learning]
created: 2026-08-27
aliases: [Reinforcement Learning MOC, RL Basics]
---

# RL MOC

> Parent: [[Home]] · The map of the vault's reinforcement-learning knowledge. Hub notes in **bold**. *(All sections written.)*

## 🧱 Foundations — one pipeline, four notes

- **[[Reinforcement Learning]]** — ⭐ **start here**: the hub — the ACT→EVALUATE→IMPROVE loop, which note owns which stage, planning vs learning, value-based vs policy-based
- [[Markov Decision Process]] — *poses the problem*: states, actions, rewards, discounting, the Markov property
- [[Bellman Equation]] — *solves it on paper* when the model is known (**planning**): $V$/$Q$, contraction, value/policy iteration
- [[Temporal Difference Learning]] — *solves it from experience* when the model is missing (**learning**): sampled backups, TD(λ), the deadly triad
- [[Exploration vs Exploitation]] — *supplies the experience*: TD only learns what the policy visits — regret, UCB/Thompson, ICM → RND → Go-Explore

## 🎯 Value-based methods

- [[Q-Learning]] — sampled value iteration: the max target, off-policy, overestimation bias + Double fix
- [[Deep Q-Network]] — replay + target network vs the deadly triad; the Double → PER → Dueling → C51 → Rainbow lineage

## 🧭 Policy-gradient methods

- [[Policy Gradient]] — why go direct; the log-derivative trick derived; baselines → advantage
- [[Actor-Critic]] — critic as baseline + bootstrap; δ trains both; A3C's parallelism-instead-of-replay
- [[Generalized Advantage Estimation]] — the λ-dial applied to advantages; PPO's default estimator
- **[[PPO]]** — the workhorse: TRPO's trust region as a clipped loss; K epochs per batch; the code-level truths; gateway to RLHF
- ⚖️ [[Value-Based vs Policy-Based RL]] — the verdict map: which family owns which domain (and the SAC/TD3 hybrid middle)

## 🗺 Model-based & search

- [[Model-Based RL]] — Dyna's loop, compounding error + MBPO's fix, Dreamer's imagination training
- [[Monte Carlo Tree Search]] — UCB in a tree; AlphaGo → AlphaGo Zero → AlphaZero → MuZero; search as policy improvement

## 🤖 RL for LLMs — post-training

- **[[RLHF]]** — SFT → reward model → PPO + KL budget; the 1.3B > 175B result; the √KL law
- [[Reward Model]] — Bradley–Terry, the overoptimization scaling laws, why bigger RMs delay but never cure Goodhart
- [[DPO]] — the closed-form shortcut: RLHF as one classification loss; offline, disease included
- [[GRPO]] — the group as the baseline; critic deleted; degenerate groups, length bias, DAPO's fixes
- [[RLVR]] — verifiers replace the RM; R1-Zero's emergent reasoning; the sharpening-vs-discovery debate

## 🛠 Projects

- [[Chinese Checkers RL Bot]] — *planning*: AlphaZero-lite on a MacBook M3 Pro — method verdict (MCTS + self-play, not Q-learning/PPO), env design, rung ladder, hardware & library plan

## 🌱 Frontier — queued next

- Offline RL · Decision Transformer — RL as sequence modeling
- Multi-agent RL · self-play
- Process vs outcome reward models

## 📚 Source library

```dataview
TABLE authors, year, rating FROM "30 Sources"
WHERE contains(topics, "reinforcement-learning")
SORT year ASC
```

## Auto-collected (unfiled RL notes)

```dataview
LIST FROM "10 Notes"
WHERE contains(topics, "reinforcement-learning") AND !contains(file.outlinks, this.file.link)
```
