---
type: source
source-type: paper
authors: [Gerald Tesauro]
year: 1992
url: https://consensus.app/papers/details/52773b25eafa510eb9673c5d6e63a484/?utm_source=claude_desktop
doi: 10.1023/a:1022624705476
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Tesauro, TD-Gammon]
---

# Practical Issues in Temporal Difference Learning - TD-Gammon (1992)

(~730 citations. The companion 1994–95 papers pushed the same system to world-champion level.)

## Summary

The first application of TD(λ) to a complex, non-trivial task: a neural network learns **backgammon from self-play alone**, predicting game outcome and improving via TD errors — the empirical proof that TD + function approximation + self-play works far better than the theory of the time predicted.

## Key results

- With **zero built-in knowledge**, the network reaches strong-intermediate play — better than conventional commercial programs
- **Surpasses comparable networks trained by supervised learning on a massive human-expert dataset** — self-play TD beats imitation of experts
- Later versions (TD-Gammon 2.1, 1995) reached the level of the world's best human players and changed human opening theory

## Impact

The direct ancestor of the self-play lineage: AlphaGo/AlphaZero explicitly cite it as the proof-of-concept that bootstrapped value learning + self-play scales. Also the origin of practical wisdom on nets-as-value-functions (smoothness of the game representation mattered enormously).

## Concepts extracted

- [[Temporal Difference Learning]] — the landmark empirical result
