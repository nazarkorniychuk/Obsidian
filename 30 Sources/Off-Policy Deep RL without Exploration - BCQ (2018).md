---
type: source
source-type: paper
authors: [Scott Fujimoto, David Meger, Doina Precup]
year: 2018
url: https://consensus.app/papers/details/90c3807ebabd5cdd952ddeabdcd8ab8a/?utm_source=claude_desktop
doi: 10.48550/arxiv.1812.02900
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [BCQ, batch-constrained Q-learning, offline RL charter]
---

# Off-Policy Deep RL without Exploration - BCQ (2018)

(~2,100 citations — the paper that launched modern offline RL.)

## Summary

The **fixed-dataset** setting: learn from a batch of pre-collected data with *no further interaction*. Discovery: standard off-policy algorithms (DQN, DDPG) — nominally built for other-policy data — **fail badly when the data is uncorrelated with their own policy's distribution**. Cause: **extrapolation error** — the max/argmax queries $Q$ at state–action pairs *absent from the dataset*, where the network's guesses are unconstrained fantasy, and bootstrapping propagates the fantasy ([[Q-Learning|the overestimation machinery]], with no fresh data to ever correct it).

## Key results

- **Batch-Constrained Q-learning (BCQ):** restrict the candidate actions to ones plausible under the data (via a generative model of the batch), forcing the learner to stay near-on-policy *with respect to the dataset*. First continuous-control deep RL method that learns effectively from arbitrary fixed batches
- Defined the problem framing (and failure diagnosis) that CQL, IQL, and the offline-RL field inherit

## The structural point

Offline RL is **value-branch territory by necessity**: on-policy policy gradients require fresh rollouts from the current policy — with a fixed dataset they cannot even be *estimated*. The entire field is Q-learning-style bootstrapping plus regularization against extrapolation.

## Concepts extracted

- [[Value-Based vs Policy-Based RL]] — the offline territory
