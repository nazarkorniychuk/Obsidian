---
type: source
source-type: paper
authors: [Adrien Ecoffet, Joost Huizinga, Joel Lehman, Kenneth O. Stanley, Jeff Clune]
year: 2020
url: https://consensus.app/papers/details/b56ebc1a73215243b0aa6faf703f6ad7/?utm_source=claude_desktop
doi: 10.1038/s41586-020-03157-9
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Go-Explore, Ecoffet 2021]
---

# First Return Then Explore - Go-Explore (2020)

(~460 citations; published in *Nature* (2021).)

## Summary

Diagnoses why intrinsic-motivation agents still fail: **detachment** (the agent forgets how to reach previously found frontiers, since the novelty bonus there is already consumed) and **derailment** (stochastic exploratory actions prevent it from *reliably returning* to a frontier before exploring). **Go-Explore**: maintain an archive of visited states, *first return* to a promising archived state (deterministically or via a goal-conditioned policy), *then explore* from it.

## Key results

- **Solves all previously unsolved hard-exploration Atari games**; surpasses SOTA on all of them, with orders-of-magnitude gains on Montezuma's Revenge and Pitfall
- Works on a sparse-reward robotic pick-and-place task; goal-conditioned variant handles stochasticity throughout training

## Impact

Showed that exploration's bottleneck can be **memory and reliable returnability**, not the novelty signal itself — bonuses answer "what's new?" but not "can I get back there?".

## Concepts extracted

- [[Exploration vs Exploitation]] — detachment/derailment, archive-based exploration
