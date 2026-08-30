---
type: source
source-type: paper
authors: [Julian Schrittwieser, Ioannis Antonoglou, et al. (DeepMind)]
year: 2019
url: https://consensus.app/papers/details/85dfeecc7252571099853de484780658/?utm_source=claude_desktop
doi: 10.1038/s41586-020-03051-4
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [MuZero, Schrittwieser 2019]
---

# Mastering Atari Go Chess Shogi - MuZero (2019)

(~2,700 citations, *Nature*.)

## Summary

Removes AlphaZero's last requirement — **the rules**. MuZero *learns* a model to plan with, but not a model of the world: a latent model trained to predict only the three things planning needs — **the policy, the value, and the reward**. The model's latent states never need to reconstruct observations; they only need to make the *search* work ("value-equivalent" modeling).

## Key results

- **Matched AlphaZero's superhuman performance** in Go, chess, and shogi *without being told the rules*
- **State of the art on Atari-57** — the domain where model-based planning had historically failed — using the same algorithm

## Impact

Closed the loop between [[Model-Based RL|model-based]] and model-free RL: a model is worth learning even if it predicts nothing about the world *except what planning consumes*. Descendant [[Mastering Atari with Limited Data - EfficientZero (2021)|EfficientZero]] made the same recipe extremely sample-efficient.

## Concepts extracted

- [[Monte Carlo Tree Search]] — planning with a learned model
- [[Model-Based RL]] — value-equivalent models
