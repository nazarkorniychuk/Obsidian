---
type: source
source-type: paper
authors: [Max Schwarzer, Johan Obando-Ceron, Aaron Courville, Marc Bellemare, Rishabh Agarwal, Pablo Samuel Castro]
year: 2023
url: https://consensus.app/papers/details/7a90f81c20c55a46b77e61f57ebec1c8/?utm_source=claude_desktop
doi: 10.48550/arxiv.2305.19452
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [BBF, Schwarzer 2023]
---

# Bigger Better Faster - BBF (2023)

(~170 citations.)

## Summary

A **value-based** agent achieving **super-human performance on the Atari 100k benchmark** — ~2 hours of gameplay experience, the canonical low-data regime. The recipe: *scale the value network* (something long thought not to help in RL) plus the design choices that make scaling sample-efficient (high replay ratio with periodic network resets, n-step annealing, and the modern value-based toolkit).

## Why it's in the vault

The clean evidence point that **the low-data discrete-action regime belongs to the value branch**: when every transition is precious, replay-based value learning squeezes it hardest, and the strongest Atari-100k model-free agents have consistently been value-based (Data-Efficient Rainbow → SPR → BBF lineage). Human-level Atari efficiency was reached with *no policy gradient in sight*.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — who owns the low-data regime
- [[Deep Q-Network]] — the lineage continues
