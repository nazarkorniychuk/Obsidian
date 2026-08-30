---
type: source
source-type: paper
authors: [Tom Schaul, John Quan, Ioannis Antonoglou, David Silver]
year: 2015
url: https://consensus.app/papers/details/6173873b2dab5d1dbe0da86da811feb9/?utm_source=claude_desktop
doi: 10.48550/arxiv.1511.05952
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [PER, Schaul 2015]
---

# Prioritized Experience Replay (2015)

(~4,600 citations.)

## Summary

Uniform replay wastes gradient steps: it replays transitions at the frequency they were *experienced*, not the frequency they're *useful*. **PER** samples transitions with probability proportional to their (stochastically prioritized) **TD error** $|\delta|^\alpha$ — "replay what you're most wrong about" — and corrects the resulting distribution shift with **importance-sampling weights** annealed over training.

## Key results

- DQN + PER outperforms uniform-replay DQN on **41 of 49** Atari games — a new state of the art at the time
- The two knobs: α (how aggressive the prioritization) and β (how much IS correction), with sum-tree implementation making sampling $O(\log N)$

## Caveats (later work)

TD-error priority chases *noise* as well as signal (stochastic rewards keep high δ forever — the replay-buffer cousin of the noisy-TV problem from [[Exploration vs Exploitation]]), and follow-up studies find PER's gains less consistent outside Atari and with actor-critics.

## Concepts extracted

- [[Deep Q-Network]] — the replay-efficiency item of the lineage
