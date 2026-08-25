---
type: concept
topics: [deep-learning, optimization]
status: evergreen
created: 2026-08-25
aliases: [SGD, stochastic gradient descent, GD]
---

# Gradient Descent

Minimize a loss by stepping opposite its gradient:

$$\theta_{t+1} = \theta_t - \eta \, \nabla_\theta L(\theta_t)$$

with learning rate $\eta$ — **the single most important hyperparameter in deep learning**. Gradients come from [[Backpropagation]]; essentially all of deep learning is this loop plus engineering.

## SGD — the noise is a feature

Full-batch gradients are unaffordable and unnecessary: estimate $\nabla L$ on a **mini-batch**. Consequences:

- Cost per step becomes independent of dataset size — the property that makes internet-scale training possible
- The gradient noise acts as **implicit regularization** (noise helps escape sharp regions) and interacts with batch size: larger batches → less noise → larger stable $\eta$; beyond a task-dependent **critical batch size**, adding data parallelism stops buying wall-clock (diminishing returns per step)
- Sampling without replacement per epoch (shuffling) in practice; LLM pre-training is typically ~1 epoch — every batch is fresh data, and classical overfitting machinery ([[Dropout (2014)|dropout]]) largely retired

## Momentum

$$v_{t+1} = \mu v_t - \eta \nabla L(\theta_t), \qquad \theta_{t+1} = \theta_t + v_{t+1}$$

A velocity accumulator (typically $\mu = 0.9$): consistent gradient directions compound (up to $1/(1-\mu) = 10\times$ effective step), oscillating directions cancel — physically, a heavy ball in a viscous medium, bringing curvature directions closer to critical damping. **The result that established it:** SGD + momentum + well-designed [[Weight Initialization|initialization]] matches second-order Hessian-Free optimization on deep/recurrent nets; init and momentum interact multiplicatively (bad init cannot be rescued by momentum), and prior "deep nets are untrainable" failures were initialization failures ([[On the Importance of Initialization and Momentum (2013)|Sutskever 2013]]). Nesterov variant: evaluate the gradient at the look-ahead point $\theta + \mu v$.

**The honest scope:** in the small-learning-rate, *noise-dominated* regime, momentum provably ≈ plain SGD — its benefit concentrates where **curvature** is the enemy (large stable LRs, big batches — i.e., LLM pre-training), not where noise is ([[The Marginal Value of Momentum (2023)|Wang 2023]]).

## What theory does and doesn't guarantee

- **Convex case:** clean rates ($O(1/t)$ strongly convex, accelerated $O(1/t^2)$ with momentum); global optimum guaranteed under [[Convexity]]
- **Neural networks are non-convex** — yet GD works. Partial explanations: over-parameterization makes most local minima good; in the infinite-width limit training becomes solvable kernel regression along [[Neural Tangent Kernel]] principal components (fastest along top eigendirections — a derivation of early stopping); per-direction learning speeds = the condition-number story your [[GLU Variants]] and MoE notes use
- Loss-landscape conditioning is *the* lever: architecture changes that "just train better" (gating, normalization, residuals) act by reshaping the spectrum GD descends — see [[The Devil is in the Condition Numbers (2026)|Lyu 2026]], [[On Layer Normalization in the Transformer Architecture (2020)|Xiong 2020]]

## Where it sits today

Raw SGD(+momentum) still rules ConvNets; **transformers train poorly with it** — heavy-tailed, anisotropic gradient noise across parameter types (embeddings vs LN gains vs attention) demands per-parameter adaptive scaling → [[Adam Optimizer]] is the de-facto LLM optimizer, with SGD the baseline it's measured against.

## Related

- Gradients from [[Backpropagation]]; adaptive successor [[Adam Optimizer]]
- $\eta$ scheduling, warmup, and weight decay live in [[Adam Optimizer]]
- Convergence-speed theory: [[Neural Tangent Kernel]]; guarantees under [[Convexity]]
- Initialization interaction: [[Weight Initialization]]

## Sources

- [[On the Importance of Initialization and Momentum (2013)]] — momentum + init charter
- [[The Marginal Value of Momentum (2023)]] — momentum's boundary conditions
- [[Neural Tangent Kernel - Jacot (2018)]] — solvable-limit dynamics
- [[Dropout (2014)]] — the regularization era SGD noise partly replaced

---
Part of the foundations cluster.
