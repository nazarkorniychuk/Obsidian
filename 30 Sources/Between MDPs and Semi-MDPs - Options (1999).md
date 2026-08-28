---
type: source
source-type: paper
authors: [Richard S. Sutton, Doina Precup, Satinder Singh]
year: 1999
url: https://consensus.app/papers/details/16e26fe19d575062ae0756c2636c8325/?utm_source=claude_desktop
doi: 10.1016/s0004-3702(99)00052-1
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Options paper, Sutton Precup Singh 1999]
---

# Between MDPs and Semi-MDPs - Options (1999)

(~4,400 citations — the temporal-abstraction charter.)

## Summary

Extends the action concept to **options**: closed-loop policies with initiation sets and termination conditions ("go to the door," "pick up the object") usable interchangeably with primitive actions. Core theorem: a set of options over an MDP constitutes a **semi-Markov decision process (SMDP)**, so planning (value iteration) and learning (Q-learning) transfer wholesale to temporally extended actions.

## Key results

- Options can be **interrupted** during execution to do better than the planned option sequence
- **Intra-option** methods learn about an option from fragments of its execution — no need to run it to termination
- Subgoal-based option improvement links hierarchy to learning

## Impact

The formal foundation of hierarchical RL; the reason "act at multiple timescales" needs no new theory, only a lift from MDP to SMDP.

## Concepts extracted

- [[Markov Decision Process]] — the hierarchy extension
