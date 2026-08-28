---
type: source
source-type: paper
authors: [John N. Tsitsiklis, Benjamin Van Roy]
year: 1997
url: https://consensus.app/papers/details/6454ba41ef6c583da7583227f9fb81be/?utm_source=claude_desktop
doi: 10.1109/9.580874
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-27
aliases: [Tsitsiklis & Van Roy 1997, TvR 1997]
---

# An Analysis of Temporal-Difference Learning with Function Approximation (1997)

(~1,800 citations across versions — the convergence/divergence charter for TD.)

## Summary

The definitive positive *and* negative result for TD(λ) with **linear** function approximation, trained **on-policy** along a single trajectory of an irreducible aperiodic Markov chain.

## Key results

- **Convergence w.p.1** to a unique fixed point $\theta^*$, with the approximation-error bound
$$\|V_{\theta^*} - V^\pi\|_D \;\le\; \frac{1-\lambda\gamma}{1-\gamma}\,\|\Pi V^\pi - V^\pi\|_D$$
(distance measured under the stationary distribution $D$; $\Pi$ = projection). Larger λ → tighter bound; λ=1 recovers the best linear fit
- **On-policy sampling is essential**: if updates are weighted by any distribution other than the chain's own stationary distribution, TD **can diverge** — the precise reconciliation of prior positive and negative reports
- **Nonlinear approximators can diverge** even on-policy (explicit counterexample)

## Impact

Named the failure regime that Sutton & Barto later called the **deadly triad** (approximation + bootstrapping + off-policy). Every deep-RL stabilizer (target networks, replay near-on-policy, gradient-TD methods) is a response to this paper's negative half.

## Concepts extracted

- [[Temporal Difference Learning]] — the convergence boundary
