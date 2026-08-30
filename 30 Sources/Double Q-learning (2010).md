---
type: source
source-type: paper
authors: [Hado van Hasselt]
year: 2010
url: https://consensus.app/papers/details/7c04ec3812255a3297bf8f10ab26b8cb/?utm_source=claude_desktop
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [van Hasselt 2010, double estimator]
---

# Double Q-learning (2010)

(~1,800 citations.)

## Summary

Identifies why Q-learning **performs very poorly in some stochastic environments**: it uses the *maximum estimated* action value as a stand-in for the *maximum expected* action value — and a max over noisy estimates is positively biased (it preferentially picks whichever estimate got lucky). **Double Q-learning**: keep two independent estimators $Q^A, Q^B$; use one to *select* the best action and the other to *evaluate* it, breaking the luck-selection correlation.

## Key results

- The **double estimator** removes the overestimation (it can instead mildly *underestimate* the maximum expected value)
- Double Q-learning **converges to the optimal policy** (same style of proof as Watkins), and dramatically outperforms Q-learning in environments with high-variance rewards where standard Q-learning fails

## Impact

The template fix for maximization bias everywhere: [[Deep RL with Double Q-Learning - DDQN (2015)|Double DQN]], the twin critics of TD3/SAC in continuous control, and the standard first diagnosis when a value-based agent's estimates balloon.

## Concepts extracted

- [[Q-Learning]] — the overestimation mechanism and its fix
