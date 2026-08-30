---
type: source
source-type: paper
authors: [Richard S. Sutton, David McAllester, Satinder Singh, Yishay Mansour]
year: 1999
url: https://consensus.app/papers/details/2a0d23efd81255acab199ce38ce8caa8/?utm_source=claude_desktop
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [policy gradient theorem, Sutton 1999 PG]
---

# Policy Gradient Methods with Function Approximation (1999)

(~7,900 citations — the policy gradient theorem.)

## Summary

The theoretical charter for the policy-based branch. Motivation stated bluntly: approximating a value function and deriving the policy from it had "proven theoretically intractable" (the deadly-triad era); instead, represent the policy by **its own function approximator** and update along the gradient of expected reward.

## Key results

- **The policy gradient theorem:** $\nabla J(\theta)$ can be written as an expectation over states and actions involving only $\nabla_\theta \log \pi_\theta$ and $Q^\pi$ (or the advantage) — crucially, **no term for how the state distribution shifts with θ**, which is what makes sampled estimation possible at all
- **Compatible function approximation:** conditions under which plugging a *learned* value estimate into the gradient keeps it unbiased
- **First convergence proof** of a policy-iteration scheme with arbitrary differentiable function approximation — to a *locally* optimal policy. Contrast with the value-based side, where FA breaks the guarantees

## Concepts extracted

- [[Policy Gradient]] — the theorem
- [[Actor-Critic]] — the compatible-critic justification
