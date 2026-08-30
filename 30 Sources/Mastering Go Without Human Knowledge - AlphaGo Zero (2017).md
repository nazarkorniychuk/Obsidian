---
type: source
source-type: paper
authors: [David Silver, Julian Schrittwieser, et al. (DeepMind)]
year: 2017
url: https://consensus.app/papers/details/4ff267b2161c5bf39b7b70e9879ddb47/?utm_source=claude_desktop
doi: 10.1038/nature24270
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [AlphaGo Zero, Silver 2017]
---

# Mastering Go Without Human Knowledge - AlphaGo Zero (2017)

(~10,600 citations, *Nature*.)

## Summary

Removes the human data: starting **tabula rasa** from random play, with nothing but the rules, a single network (policy + value heads) learns purely from self-play. The key loop: **MCTS acts as a policy-improvement operator** — the search, guided by the current network, produces *better* move probabilities than the raw network; the network is then trained to predict the search's choices and the games' winners; the improved network makes the next search stronger.

## Key results

- **Beat the champion-defeating AlphaGo 100–0**
- Simpler than its predecessor (one network, no rollouts, no human features) yet far stronger — human knowledge had been a *ceiling*, not a floor

## Impact

The cleanest existing demonstration of the [[Reinforcement Learning|generalized-policy-iteration loop]]: search = IMPROVE, network training = EVALUATE/compress, self-play = ACT. Generalized to chess/shogi as [[A General RL Algorithm - AlphaZero (2018)|AlphaZero]].

## Concepts extracted

- [[Monte Carlo Tree Search]] — search as policy improvement
