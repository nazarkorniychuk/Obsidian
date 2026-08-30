---
type: source
source-type: paper
authors: [Ronald J. Williams]
year: 1992
url: https://consensus.app/papers/details/611d688477a457189e315729fc5897f8/?utm_source=claude_desktop
doi: 10.1023/a:1022672621406
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [REINFORCE, Williams 1992]
---

# Simple Statistical Gradient-Following Algorithms - REINFORCE (1992)

(~10,600 citations — the founding paper of policy-gradient RL.)

## Summary

Introduces the **REINFORCE** family: algorithms for networks containing stochastic units that adjust weights **along the gradient of expected reinforcement** — *without ever computing or even storing a gradient estimate explicitly*. The core identity is the score-function/log-derivative trick: reward times the gradient of the log-probability of what was done is an unbiased estimate of the performance gradient.

## Key results

- Proves the gradient-following property for immediate-reinforcement tasks and limited delayed-reinforcement settings
- Introduces the **baseline** — a subtracted comparison term that leaves the gradient unbiased while reducing variance
- Shows natural integration with backpropagation: the stochastic unit's contribution is $(\text{reward} - \text{baseline}) \cdot \nabla \log p(\text{action})$

## Impact

Every policy-gradient method — actor-critic, TRPO, [[PPO]], and the RLHF objectives descended from them — is a variance-reduced, trust-regioned REINFORCE. The "weighted log-likelihood" structure is also why policy gradients port so naturally to LLMs.

## Concepts extracted

- [[Policy Gradient]] — the founding algorithm and the baseline idea
