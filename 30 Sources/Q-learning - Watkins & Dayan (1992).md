---
type: source
source-type: paper
authors: [Christopher J.C.H. Watkins, Peter Dayan]
year: 1992
url: https://consensus.app/papers/details/ae18fbb53bc35bbe89b045561bc57357/?utm_source=claude_desktop
doi: 10.1007/bf00992698
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-27
aliases: [Watkins 1992, Q-learning paper]
---

# Q-learning - Watkins & Dayan (1992)

(~12,700 citations. Q-learning itself is from Watkins' 1989 PhD thesis; this is the convergence proof.)

## Summary

**Q-learning**: learn the optimal action-value function directly, off-policy, by the update
$$Q(s,a) \leftarrow Q(s,a) + \alpha\big[r + \gamma \max_{a'} Q(s',a') - Q(s,a)\big]$$
— an incremental, model-free form of value iteration.

## Key result

**Convergence theorem**: tabular Q-learning converges to $Q^*$ with probability 1, provided (i) every state–action pair is sampled infinitely often and (ii) step sizes satisfy the Robbins–Monro conditions ($\sum \alpha = \infty$, $\sum \alpha^2 < \infty$). The behavior policy can be *anything* satisfying (i) — this is what "off-policy" means.

## Impact

The most-used RL algorithm of the tabular era and the core of [[Deep Q-Network|DQN]]. Its `max` operator is also the source of two chronic pathologies: **overestimation bias** (fixed by Double Q-learning) and instability under function approximation (the off-policy leg of the deadly triad — see [[An Analysis of Temporal-Difference Learning with Function Approximation (1997)]]).

## Concepts extracted

- [[Temporal Difference Learning]] — TD control, off-policy
- [[Q-Learning]] — the algorithm (planned note)
