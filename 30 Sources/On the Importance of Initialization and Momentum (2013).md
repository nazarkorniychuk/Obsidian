---
type: source
source-type: paper
authors: [Ilya Sutskever, James Martens, George Dahl, Geoffrey Hinton]
year: 2013
url: https://consensus.app/papers/details/567cbdd1ddd75f3e9eb0b5b8d328c287/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Sutskever 2013, momentum paper]
---

# On the Importance of Initialization and Momentum (2013)

(ICML 2013, ~5,300 citations.)

## Summary

Deep and recurrent nets were considered nearly untrainable by first-order methods. This paper showed **SGD with momentum** — $v_{t+1} = \mu v_t - \eta \nabla L$, $\theta_{t+1} = \theta_t + v_{t+1}$ — reaches the performance of second-order Hessian-Free optimization, *provided* two things are right: **well-designed random initialization** and a **schedule that ramps the momentum coefficient** $\mu$ up slowly. Also popularized the Nesterov variant (gradient evaluated at the look-ahead point $\theta + \mu v$).

## Key results

- Poorly initialized networks **cannot be rescued by momentum**; well-initialized networks degrade markedly without it — the two interact multiplicatively
- Conclusion that redirected the field: prior failures to train deep nets were **initialization failures**, not fundamental barriers; "carefully tuned momentum methods suffice" — no second-order machinery needed

## Why it's in the vault

The empirical charter of the modern recipe (good init + first-order momentum method), and the reason $\beta_1$ exists in [[Adam - A Method for Stochastic Optimization (2014)|Adam]]. Caveat from later theory: in small-learning-rate, noise-dominated regimes momentum's benefit provably shrinks (Wang 2023) — its value concentrates where curvature, not noise, is the enemy.

## Concepts extracted

- [[Gradient Descent]] — momentum; [[Weight Initialization]] — the interaction
