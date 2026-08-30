---
type: source
source-type: paper
authors: [Tuomas Haarnoja, Aurick Zhou, Pieter Abbeel, Sergey Levine]
year: 2018
url: https://consensus.app/papers/details/b9a86a6c778153eebe09770a0804ed1f/?utm_source=claude_desktop
doi: 10.48550/arxiv.1801.01290
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [SAC, Haarnoja 2018]
---

# Soft Actor-Critic (2018)

(~12,100 citations + ~3,300 for the applications version — the default off-policy continuous-control algorithm.)

## Summary

Targets the two failures limiting deep RL in the real world: **very high sample complexity** (on-policy methods discard data) and **brittle convergence** requiring meticulous tuning (off-policy DDPG-style methods). SAC: an **off-policy actor-critic in the maximum-entropy framework** — the actor maximizes expected reward *plus* policy entropy ("succeed while acting as randomly as possible"), with replay buffer and twin soft-Q critics.

## Key results

- **State of the art on continuous-control benchmarks, outperforming both prior on-policy and off-policy methods** — in sample efficiency *and* asymptotic performance (applications version)
- Distinctively **stable across random seeds** — rare among off-policy methods
- Extended version: learned real-world **quadruped locomotion** and **dexterous-hand manipulation** — the sample efficiency is what makes real-hardware training feasible at all

## Impact

Where samples are expensive and actions continuous (robotics), SAC-class off-policy actor-critic — Q-learning machinery with a stochastic actor as argmax-solver — is the default over PPO. The entropy term also unifies the [[Exploration vs Exploitation|Boltzmann-exploration]] thread with value learning.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — who owns real-robot continuous control
