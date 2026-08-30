---
type: source
source-type: paper
authors: [Hado van Hasselt, Arthur Guez, David Silver]
year: 2015
url: https://consensus.app/papers/details/94ef46fd071c5f1187f3d53f7eb6ea3e/?utm_source=claude_desktop
doi: 10.1609/aaai.v30i1.10295
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Double DQN, DDQN, van Hasselt 2015]
---

# Deep RL with Double Q-Learning - DDQN (2015)

(~9,400 citations.)

## Summary

Answers three open questions about Q-learning's overestimation **all affirmatively**: it is *common* in practice (DQN substantially overestimates values in many Atari games), it *harms performance*, and it *can be prevented*. The fix adapts [[Double Q-learning (2010)|tabular Double Q-learning]] to DQN with zero extra networks: use the **online network to select** the argmax action and the **existing target network to evaluate** it:

$$y = r + \gamma\, Q_{\text{target}}\big(s',\, \arg\max_{a'} Q_{\text{online}}(s', a')\big)$$

## Key results

- Measured value estimates drop back toward truth (overestimation confirmed as the cause, not a symptom)
- **Much better scores on several games** — lower bias translated directly into better policies

## Impact

Became a default component of every subsequent DQN variant (baked into [[Rainbow - Combining Improvements in Deep RL (2017)|Rainbow]]); the demonstration that estimator *bias* — not just variance — is a first-order concern in deep value learning.

## Concepts extracted

- [[Deep Q-Network]] — the first item of the improvement lineage
- [[Q-Learning]] — overestimation at scale
