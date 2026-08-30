---
type: source
source-type: paper
authors: [Chao Yu, Akash Velu, Eugene Vinitsky, et al.]
year: 2021
url: https://consensus.app/papers/details/c4ecc5e52c17583f8a2e8f0093c1bfc9/?utm_source=claude_desktop
doi: 10.52202/068431-1787
topics: [reinforcement-learning]
status: processed
rating: 3
created: 2026-08-29
aliases: [MAPPO, Yu 2021]
---

# Surprising Effectiveness of PPO in Cooperative Multi-Agent Games (2021)

(~2,700 citations.)

## Summary

Challenges the belief that PPO is too sample-inefficient for multi-agent RL (where off-policy methods dominated). Careful study of PPO-based agents (**MAPPO**) on four cooperative testbeds: particle worlds, StarCraft multi-agent challenge, Google Research Football, Hanabi.

## Key results

- PPO achieves **competitive or superior results to off-policy methods in both final returns *and* sample efficiency** — with minimal hyperparameter tuning and no domain-specific modifications
- Ablations identify the implementation/hyperparameter factors doing the work — the [[Implementation Matters - PPO vs TRPO (2020)|code-level-details]] theme again

## Why it's in the vault

Evidence for the boundary of the sample-efficiency argument: where environments are simulated and parallelizable, well-tuned PPO holds ground even on the off-policy family's home turf of sample efficiency.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — PPO's side of the ledger
- [[PPO]] — robustness across settings
