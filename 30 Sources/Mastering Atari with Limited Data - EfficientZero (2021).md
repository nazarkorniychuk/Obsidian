---
type: source
source-type: paper
authors: [Weirui Ye, Shaohuai Liu, Thanard Kurutach, Pieter Abbeel, Yang Gao]
year: 2021
url: https://consensus.app/papers/details/66e9d3b7e2d25cf5bfd65e1cf4b5c311/?utm_source=claude_desktop
doi: 10.48550/arxiv.2111.00210
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [EfficientZero, Ye 2021]
---

# Mastering Atari with Limited Data - EfficientZero (2021)

(~340 citations.)

## Summary

A sample-efficient **model-based** agent built on MuZero ([[Monte Carlo Tree Search|MCTS]] + learned model): adds self-supervised consistency for the latent model and corrections for off-policy value targets.

## Key results

- **194.3% mean / 109.0% median human performance on Atari 100k** — with only **two hours** of real-time game experience; the first super-human Atari result at that data scale
- Performance close to DQN at 200M frames while consuming **500× less data**

## Why it's in the vault

Completes the sample-efficiency ladder: on-policy PG (data-hungriest) → off-policy value-based ([[Bigger Better Faster - BBF (2023)|BBF]]) → **model-based + search**, which wins when data is scarcest — learn the world, then practice inside it. The third family the value/policy dichotomy leaves out.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — the model-based caveat
- [[Model-Based RL]] · [[Monte Carlo Tree Search]] — planned notes
