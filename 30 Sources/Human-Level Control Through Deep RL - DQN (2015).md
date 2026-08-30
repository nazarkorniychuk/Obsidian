---
type: source
source-type: paper
authors: [Volodymyr Mnih, Koray Kavukcuoglu, David Silver, et al. (DeepMind)]
year: 2015
url: https://consensus.app/papers/details/99b553e2467e50c49517e20f36e999eb/?utm_source=claude_desktop
doi: 10.1038/nature14236
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [DQN, Nature DQN, Mnih 2015]
---

# Human-Level Control Through Deep RL - DQN (2015)

(~33,000 citations, *Nature* — the paper that launched the deep-RL era.)

## Summary

The **Deep Q-Network**: Q-learning with a convolutional network reading raw pixels, made stable by two mechanisms — **experience replay** (store 1M transitions, train on random minibatches → decorrelates data, reuses experience) and a **target network** (frozen copy providing the bootstrap target, synced every ~10k steps → the network stops chasing itself).

## Key results

- **One algorithm, one architecture, one hyperparameter set** across **49 Atari games**, input = pixels + score only
- Performance **comparable to a professional human games tester** across the suite; surpassed all previous algorithms
- Recipe details that became standard: 84×84×4 stacked grayscale frames (the [[Markov Decision Process|POMDP]] frame-stacking fix), reward clipping to [−1, 1], ε-greedy annealed 1 → 0.1, RMSProp, ~50M frames of training (≈ 38 days of game experience per game)

## Impact

Demonstrated that the [[Temporal Difference Learning#The deadly triad|deadly triad]] can be *engineered around* rather than solved: replay ≈ tempering the off-policy leg, target network ≈ tempering the bootstrapping leg. Every subsequent value-based deep agent is a descendant.

## Concepts extracted

- [[Deep Q-Network]] — the algorithm and both stabilizers
