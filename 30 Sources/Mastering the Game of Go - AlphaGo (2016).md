---
type: source
source-type: paper
authors: [David Silver, Aja Huang, et al. (DeepMind)]
year: 2016
url: https://consensus.app/papers/details/d0f2133b6e5b5b8ea03b29a6a2e51638/?utm_source=claude_desktop
doi: 10.1038/nature16961
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [AlphaGo, Silver 2016]
---

# Mastering the Game of Go - AlphaGo (2016)

(~19,200 citations, *Nature*.)

## Summary

Combines [[Monte Carlo Tree Search|MCTS]] with two deep networks: a **policy network** (trained first by supervised learning on human expert games, then by [[Policy Gradient|policy-gradient]] self-play) proposing moves to narrow the search, and a **value network** (trained on self-play outcomes) evaluating positions to truncate rollouts.

## Key results

- The networks *alone*, with no search, already played at the level of state-of-the-art MCTS programs
- With search: **99.8% win rate vs other Go programs**; defeated the European champion **5–0** — the first professional defeat in full-size Go, "a feat previously thought to be at least a decade away" (and beat world champion Lee Sedol 4–1 shortly after publication)

## Impact

The template "search proposes, networks evaluate, self-play improves" became the canonical neural-search architecture — and the direct descendant of [[Practical Issues in Temporal Difference Learning - TD-Gammon (1992)|TD-Gammon]]'s self-play idea.

## Concepts extracted

- [[Monte Carlo Tree Search]] — the neural-MCTS marriage
