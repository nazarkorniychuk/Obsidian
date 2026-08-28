---
type: moc
topics: [reinforcement-learning]
created: 2026-08-27
aliases: [Reinforcement Learning MOC, RL Basics]
---

# RL MOC

> Parent: [[Home]] · The map of the vault's reinforcement-learning knowledge. Hub notes in **bold**. *(Foundations written; other sections are planned notes, filled in as we go.)*

## 🧱 Foundations — the formalism

- **[[Markov Decision Process]]** — states, actions, rewards, transitions, discounting; the problem statement
- [[Bellman Equation]] — $V^\pi$, $Q^\pi$, optimality, contraction; value/policy iteration (PI = Newton)
- [[Temporal Difference Learning]] — bootstrapping, TD(λ), the deadly triad, TD-Gammon
- [[Exploration vs Exploitation]] — regret bounds, UCB, Thompson; ICM → RND → Go-Explore

## 🎯 Value-based methods

- [[Q-Learning]] — off-policy TD control; convergence conditions
- [[Deep Q-Network]] — replay buffer, target networks, the deadly triad; Double/Dueling/Rainbow lineage

## 🧭 Policy-gradient methods

- [[Policy Gradient]] — REINFORCE, the log-derivative trick, baselines and variance reduction
- [[Actor-Critic]] — A2C/A3C; [[Generalized Advantage Estimation]]
- **[[PPO]]** — the workhorse: clipped surrogate objective, TRPO lineage, implementation details that matter

## 🗺 Model-based & search

- [[Model-Based RL]] — world models, Dyna, planning vs learning
- [[Monte Carlo Tree Search]] — AlphaGo → AlphaZero → MuZero lineage

## 🤖 RL for LLMs — post-training

- **[[RLHF]]** — reward model from preferences + PPO on the policy; the InstructGPT recipe
- [[Reward Model]] — Bradley–Terry preferences, reward hacking, overoptimization scaling laws
- [[DPO]] — preference optimization without a reward model or RL loop
- [[GRPO]] — group-relative advantages, critic-free; the DeepSeek-R1 recipe
- [[RLVR]] — verifiable rewards; reasoning training, test-time compute

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
