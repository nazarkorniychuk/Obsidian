---
type: source
source-type: paper
authors: [Volodymyr Mnih, Koray Kavukcuoglu, David Silver, et al. (DeepMind)]
year: 2013
url: https://consensus.app/papers/details/aaead32e18e05cbebe0dc7330bb4a305/?utm_source=claude_desktop
doi: 10.48550/arxiv.1312.5602
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [DQN workshop paper, Mnih 2013]
---

# Playing Atari with Deep Reinforcement Learning (2013)

(~14,000 citations — the NIPS workshop paper that started deep RL.)

## Summary

First deep learning model to learn control policies **directly from high-dimensional sensory input** (raw pixels) with RL: a convolutional network trained by a variant of Q-learning, stabilized with **experience replay**. Same architecture and algorithm applied to seven Atari 2600 games with no per-game adjustment.

## Key results

- Outperforms all previous approaches on **6 of 7** games; surpasses a human expert on **3** (Breakout, Enduro, Pong)
- Established the recipe: replay buffer + ε-greedy + reward clipping + frame stacking

## Impact

The proof-of-concept that convinced the field pixels-to-actions RL was possible; the [[Human-Level Control Through Deep RL - DQN (2015)|Nature version]] scaled it to 49 games and added the target network.

## Concepts extracted

- [[Deep Q-Network]] — the original recipe
