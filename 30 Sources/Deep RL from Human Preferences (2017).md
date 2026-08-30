---
type: source
source-type: paper
authors: [Paul Christiano, Jan Leike, Tom Brown, et al.]
year: 2017
url: https://consensus.app/papers/details/1e12563e29bc58c0a96fdde5e464b2fc/?utm_source=claude_desktop
doi: 10.48550/arxiv.1706.03741
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [Christiano 2017, RLHF origin]
---

# Deep RL from Human Preferences (2017)

(~5,900 citations — the founding paper of RLHF.)

## Summary

Replaces the hand-written reward function with **human preferences between pairs of trajectory segments**: show a human two short clips of agent behavior, ask which is better, fit a reward model to the comparisons ([[Reward Model|Bradley–Terry]]), and train the policy on the learned reward — asking humans about only the most informative pairs.

## Key results

- Solves Atari games and simulated locomotion **with feedback on less than 1% of interactions** — human oversight cheap enough for real RL systems
- Trains behaviors with **no reward function at all**: a simulated hopper learns a backflip from ~an hour of human comparisons — behavior too fuzzy to specify numerically, easy to *recognize*
- Established the pairwise-comparison interface: humans are far more reliable at ranking two things than scoring one

## Impact

The complete RLHF blueprint — preference collection → reward model → RL — five years before it aligned GPT-3 ([[Training LMs to Follow Instructions - InstructGPT (2022)|InstructGPT]]).

## Concepts extracted

- [[RLHF]] — the original recipe
- [[Reward Model]] — learning R̂ from comparisons
