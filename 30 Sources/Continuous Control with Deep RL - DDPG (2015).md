---
type: source
source-type: paper
authors: [Timothy Lillicrap, Jonathan Hunt, et al. (DeepMind)]
year: 2015
url: https://consensus.app/papers/details/cba7cc43445b5d2aa7f1b4dfa1cc1882/?utm_source=claude_desktop
doi: 10.48550/arxiv.1509.02971
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [DDPG, Lillicrap 2015]
---

# Continuous Control with Deep RL - DDPG (2015)

(~15,900 citations.)

## Summary

"DQN for continuous actions": since $\max_a Q(s,a)$ can't be enumerated over continuous actions, train a **deterministic actor** $\mu(s)$ to *be* the argmax — updated by the gradient of $Q$ through the action — while the critic learns by Q-learning with DQN's full toolkit (replay buffer, target networks, here as soft/Polyak updates).

## Key results

- One algorithm/architecture/hyperparameter set solves **20+ simulated physics tasks** (cartpole swing-up, dexterous manipulation, legged locomotion, driving), some end-to-end from pixels
- Competitive with a *planning* algorithm given full access to the dynamics and its derivatives

## Impact

Founded the off-policy continuous-control lineage: the actor here is not a policy-gradient policy but a **learned argmax-solver bolted onto Q-learning** — the structural insight behind TD3 and SAC.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — the hybrid middle
