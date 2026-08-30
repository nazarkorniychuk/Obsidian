---
type: source
source-type: paper
authors: [Volodymyr Mnih, Adrià Puigdomènech Badia, Mehdi Mirza, et al.]
year: 2016
url: https://consensus.app/papers/details/b4699938ef3a53b4b94c125459125ead/?utm_source=claude_desktop
doi: 10.48550/arxiv.1602.01783
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [A3C, A2C, Mnih 2016]
---

# Asynchronous Methods for Deep RL - A3C (2016)

(~10,300 citations.)

## Summary

Runs **many parallel actor-learners** (16 CPU threads), each interacting with its own environment copy and asynchronously updating shared parameters. The key insight: **parallelism is an alternative decorrelator to experience replay** — at any moment the workers are in different parts of different episodes, so the combined gradient stream is naturally decorrelated *without a buffer*, which means **on-policy methods become stable in deep RL**.

## Key results

- **A3C** (asynchronous advantage actor-critic, with n-step returns and an entropy bonus) surpassed the then-SOTA on Atari **in half the training time, on a multi-core CPU instead of a GPU**
- Same framework works for continuous motor control and 3D maze navigation from pixels
- The synchronous variant (**A2C**) later proved equally good — the asynchrony was an engineering convenience, the *parallel actors* were the substance

## Impact

Made deep actor-critic practical and established the on-policy template (parallel rollouts → advantage-weighted policy update + value loss + entropy bonus) that [[PPO]] inherited wholesale.

## Concepts extracted

- [[Actor-Critic]] — the deep-RL landmark
