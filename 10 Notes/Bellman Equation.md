---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [Bellman equations, value function, value iteration, policy iteration, dynamic programming]
---

# Bellman Equation

The recursive structure of value in a [[Markov Decision Process]]: the value of now = immediate reward + discounted value of next. Every value-based RL algorithm is a way of solving one of these equations from samples.

## ⚡ The four equations

| Object | Definition | Bellman form |
|---|---|---|
| $V^\pi(s)$ | $\mathbb{E}_\pi[G_t \mid s_t{=}s]$ | $V^\pi(s) = \sum_a \pi(a\|s) \sum_{s'} P(s'\|s,a)\,[r + \gamma V^\pi(s')]$ |
| $Q^\pi(s,a)$ | $\mathbb{E}_\pi[G_t \mid s_t{=}s, a_t{=}a]$ | $Q^\pi(s,a) = r(s,a) + \gamma \sum_{s'} P(s'\|s,a) \sum_{a'} \pi(a'\|s') Q^\pi(s',a')$ |
| $V^*(s)$ | $\max_\pi V^\pi(s)$ | $V^*(s) = \max_a \sum_{s'} P(s'\|s,a)\,[r + \gamma V^*(s')]$ |
| $Q^*(s,a)$ | $\max_\pi Q^\pi(s,a)$ | $Q^*(s,a) = r(s,a) + \gamma \sum_{s'} P(s'\|s,a) \max_{a'} Q^*(s',a')$ |

The expectation equations (top two) are **linear** in $V$/$Q$ — policy evaluation is a linear solve. The optimality equations (bottom two) have a **max** — nonlinear, and the actual object of RL. Given $Q^*$, acting optimally is trivial: $\pi^*(s) = \arg\max_a Q^*(s,a)$ — this is why value-based methods learn $Q$, not $V$: the argmax needs no model.

**Advantage** $A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)$: how much better than the policy's average this action is. Zero-mean under $\pi$ — the variance-reduced signal all of [[Policy Gradient|policy-gradient]] RL feeds on.

## Why a solution exists: contraction

The Bellman optimality operator $(TV)(s) = \max_a \sum_{s'}P(s'|s,a)[r+\gamma V(s')]$ is a **γ-contraction in sup-norm**:

$$\|TV - TU\|_\infty \le \gamma\, \|V - U\|_\infty$$

- **Results** ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]): by Banach's fixed-point theorem $V^*$ exists, is **unique**, and iterating $T$ from *any* start converges. Error after $k$ sweeps $\le \gamma^k\|V_0 - V^*\|_\infty$, so reaching precision $\epsilon$ takes $k \approx \tfrac{\ln(1/\epsilon)}{1-\gamma}$ sweeps — the $\tfrac{1}{1-\gamma}$ horizon tax again
- The greedy policy extracted from an approximate $V$ with $\|V - V^*\|_\infty \le \epsilon$ loses at most $\tfrac{2\gamma\epsilon}{1-\gamma}$ — approximation error is amplified by the horizon ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]])

## The two classical solvers

- **Value iteration (VI):** $V_{k+1} = TV_k$. Fixed-point iteration; global linear rate $\gamma$
- **Policy iteration (PI):** alternate *evaluate* ($V^{\pi_k}$, a linear solve) and *improve* (greedy w.r.t. $V^{\pi_k}$). Monotone improvement; terminates in finitely many steps on finite MDPs ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])
- **Result — why PI is so fast:** PI is exactly the **semismooth Newton method** on the Bellman equation → **local quadratic convergence**, while VI is plain fixed-point iteration; this explains the long-standing observation that PI needs remarkably few iterations even with enormous policy spaces ([[Dynamic Programming as Semismooth Newton (2022)|Gargiani 2022]]). PI : VI :: Newton : gradient descent
- Also solvable as a **linear program** (min $\sum_s V(s)$ s.t. $V \ge TV$; the dual optimizes over state–action occupancy measures — the formulation behind offline-RL and constrained-RL theory) ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])

## From equations to algorithms

The sampled-and-approximated versions of the two Bellman equation families generate the method zoo:

| Solve this | with samples | = |
|---|---|---|
| expectation equation for $V^\pi$ | one transition at a time | [[Temporal Difference Learning\|TD(0)]] |
| optimality equation for $Q^*$ | one transition + max | [[Q-Learning]] |
| expectation equation for $Q^\pi$, $\pi$ improving | on-policy samples | SARSA / [[Actor-Critic]] critics |

**Conclusion:** the contraction that guarantees convergence lives in **sup-norm over exact tables**. Replace tables with function approximation and samples with off-policy data, and the guarantee dissolves — the deadly-triad story picked up in [[Temporal Difference Learning]].

## Related

- [[Markov Decision Process]] — the underlying formalism
- [[Temporal Difference Learning]] — sample-based Bellman solving
- [[Monte Carlo Tree Search]] — Bellman backups on a tree, on demand

## Sources

- [[Markov Decision Processes - Puterman (1994)]] — existence, uniqueness, VI/PI/LP
- [[Dynamic Programming and Optimal Control - Bertsekas (1995)]] — error amplification bounds
- [[Dynamic Programming as Semismooth Newton (2022)]] — PI = Newton, quadratic rate
- [[Reinforcement Learning - An Introduction (1998)]] — notation and the algorithmic framing

---
Part of the RL foundations cluster — map: [[RL MOC]].
