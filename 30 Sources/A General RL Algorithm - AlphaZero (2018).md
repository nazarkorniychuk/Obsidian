---
type: source
source-type: paper
authors: [David Silver, Thomas Hubert, Julian Schrittwieser, et al. (DeepMind)]
year: 2018
url: https://consensus.app/papers/details/858c2f7e74ee52e68ce3c48eb6b57b56/?utm_source=claude_desktop
doi: 10.1126/science.aar6404
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [AlphaZero, Silver 2018]
---

# A General RL Algorithm - AlphaZero (2018)

(~4,200 citations *Science* + ~2,100 for the arXiv version.)

## Summary

Generalizes AlphaGo Zero into **one algorithm** for chess, shogi, and Go: same network architecture, same MCTS + self-play loop, no domain knowledge beyond the rules.

## Key results

- Starting from random play, reached **superhuman level in all three games within 24 hours** of training (arXiv version)
- **Convincingly defeated the world-champion program in each**: Stockfish (chess — the product of decades of hand-crafted search engineering), Elmo (shogi), and AlphaGo Zero itself
- Searches *thousands* of positions per second where Stockfish searches *tens of millions* — the network's learned evaluation replaces brute breadth with selective depth

## Impact

Ended the era of hand-crafted evaluation functions in games, and demonstrated that the self-play + search recipe is *general* — given a perfect model (the rules), one loop masters unrelated domains.

## Concepts extracted

- [[Monte Carlo Tree Search]] — the generality result
- [[Value-Based vs Policy-Based RL]] — what a free perfect model buys
