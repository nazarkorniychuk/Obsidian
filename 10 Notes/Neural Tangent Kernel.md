---
type: concept
topics: [deep-learning, optimization]
status: evergreen
created: 2026-08-24
aliases: [NTK, NTK regime, lazy training]
---

# Neural Tangent Kernel

The theoretical microscope for training dynamics: in the **infinite-width limit**, a [[Neural Network]] trained by [[Gradient Descent]] behaves as **kernel regression with a fixed kernel** — the messy non-convex problem becomes exactly solvable ([[Neural Tangent Kernel - Jacot (2018)|Jacot 2018]]).

## The object and the theorem

$$\Theta(x, x') = \nabla_\theta f_\theta(x)^\top \, \nabla_\theta f_\theta(x')$$

— the similarity of two inputs *as the network's gradients see them*. During GD, the network's function evolves by kernel gradient descent w.r.t. $\Theta$. At finite width $\Theta$ is random (init-dependent) and moves during training; **as width → ∞ it converges to a deterministic kernel and stays constant** — training the network ≡ training a linear model in a fixed feature space ("lazy training": weights barely move, features frozen at init).

## What the solvable limit buys

- **Linear dynamics:** for square loss, the function follows a linear ODE; error components decay at rates set by the **kernel's eigenvalues** — top eigendirections learn fast, small ones crawl (theoretical derivation of early stopping)
- **Condition number as the master quantity:** $\kappa = \lambda_{max}/\lambda_{min}$ measures how ravine-like the effective landscape is — the analytical handle behind results across this vault: gating shrinks $\kappa$ → SwiGLU's free win ([[The Devil is in the Condition Numbers (2026)|Lyu 2026]]); convergence guarantees tie to kernel positive-definiteness; per-direction speeds explain loss-curve shapes (plateaus = slow eigendirections finally learning)
- Init-time NTK analysis is also how variance-propagation and stability claims (e.g. [[Weight Initialization]] schemes, [[On Layer Normalization in the Transformer Architecture (2020)|pre-LN gradients]]) get formalized

## The limits of the limit

- **Lazy ≠ real:** in the NTK regime features never move — but the empirical magic of deep learning *is* feature learning ([[Learning Representations by Back-propagating Errors (1986)|hidden representations]], [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|interpretable FFN keys]], induction heads). Networks at practical scale leave the lazy regime; NTK predictions are qualitative guides, not quantitative truths
- **The parameterization decides the regime:** standard parameterization drifts toward lazy behavior as width grows; **μP is precisely the unique width-scaling that preserves feature learning at infinite width** — and its hyperparameter-transfer payoff (tune at 13M–40M, transfer to billions at ~7% tuning cost) made the theory operational ([[Tensor Programs V - muTransfer (2022)|Yang 2022]]). NTK = the solvable idealization; μP = the corrective that keeps the theory connected to practice
- Honest reading for any "proved in the NTK regime" claim in this vault (e.g. the GLU result): mechanism identification, extrapolated to finite width — treat as strong hypothesis, not theorem-about-your-model

## Related

- Idealized dynamics of [[Gradient Descent]] on a [[Neural Network]]
- Regime selection by [[Weight Initialization]]/μP; condition-number applications in [[GLU Variants]] and [[Mixture of Experts]]
- Convergence-speed language used throughout the [[Transformer]] cluster's "why it trains better" results

## Sources

- [[Neural Tangent Kernel - Jacot (2018)]] — the founding theorem
- [[Tensor Programs V - muTransfer (2022)]] — feature learning vs lazy; μTransfer
- [[The Devil is in the Condition Numbers (2026)]] — flagship application

---
Part of the foundations cluster.
