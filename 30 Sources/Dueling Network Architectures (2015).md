---
type: source
source-type: paper
authors: [Ziyu Wang, Tom Schaul, Matteo Hessel, Hado van Hasselt, Marc Lanctot, Nando de Freitas]
year: 2015
url: https://consensus.app/papers/details/72c53c90b0085045ba27f1acf468d908/?utm_source=claude_desktop
doi: 10.48550/arxiv.1511.06581
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Dueling DQN, Wang 2015]
---

# Dueling Network Architectures (2015)

(~4,500 citations.)

## Summary

An architecture change, not an algorithm change: split the Q-network head into **two streams** — one estimating the state value $V(s)$, one the per-action advantages $A(s,a)$ — recombined as

$$Q(s,a) = V(s) + \Big(A(s,a) - \tfrac{1}{|\mathcal{A}|}\textstyle\sum_{a'} A(s,a')\Big)$$

(the mean-subtraction pins down the otherwise unidentifiable $V$/$A$ split). Rationale: in many states the choice of action barely matters — a single $V$ stream can learn "this state is good/bad" from *every* transition through it, without needing per-action data.

## Key results

- **Better policy evaluation when many actions have similar values**; the value stream learns state quality even for actions never taken
- Combined with existing DQN improvements → outperformed the state of the art on Atari at publication
- Works with any underlying value-based algorithm — pure drop-in

## Impact

Made the [[Bellman Equation|V + advantage decomposition]] a standard architectural motif; a component of [[Rainbow - Combining Improvements in Deep RL (2017)|Rainbow]].

## Concepts extracted

- [[Deep Q-Network]] — the architecture item of the lineage
