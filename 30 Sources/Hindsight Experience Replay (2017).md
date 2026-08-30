---
type: source
source-type: paper
authors: [Marcin Andrychowicz, Filip Wolski, Alex Ray, et al. (OpenAI)]
year: 2017
url: https://consensus.app/papers/details/d46a95b0a2d556d0a1ad9684faf502e4/?utm_source=claude_desktop
doi: 10.48550/arxiv.1707.01495
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [HER, hindsight relabeling]
---

# Hindsight Experience Replay (2017)

(~2,800 citations.)

## Summary

The trick for **sparse binary rewards without reward engineering**: a failed trajectory toward goal A *is* a successful trajectory toward wherever you actually ended up. **Relabel** stored transitions with achieved goals, turning failures into training signal — an implicit curriculum from easy (nearby) goals to hard ones.

## Key results

- Robot-arm **pushing, sliding, pick-and-place** learned from *binary* success/failure rewards only, where the same algorithm without HER fails entirely (ablation)
- Policies trained in simulation deploy on a physical robot

## The structural point

HER **requires an off-policy algorithm** (it's combined with DQN/DDPG): relabeled transitions answer "what if my goal had been different?" — a question about data collected under *other* intentions, which only off-policy machinery can consume. On-policy policy gradients cannot use relabeled experience without breaking their estimator — goal-conditioned sparse-reward RL is value-branch territory by construction.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — the goal-relabeling territory
- [[Exploration vs Exploitation]] — curriculum from hindsight, a cousin of Go-Explore's archive
