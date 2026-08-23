---
type: concept
topics: [optimization, machine-learning, deep-learning]
status: growing
created: 2026-08-22
aliases: [GD]
---

# Gradient Descent

Iteratively minimize a function by stepping opposite its [[Gradient]] — the direction of locally steepest descent:

$$\theta_{t+1} = \theta_t - \eta \, \nabla_\theta J(\theta_t)$$

where $\eta$ is the [[Learning Rate]].

## Intuition

A ball rolling downhill in the loss landscape, taking discrete steps. Step too small → slow; too large → overshoot and diverge.

## Variants

- **Batch GD** — gradient over the full dataset (exact, expensive)
- [[Stochastic Gradient Descent]] — one sample / mini-batch (noisy, cheap, and the noise actually helps)
- [[Adam Optimizer]] — adaptive per-parameter step sizes; the practical default

## Why it matters

Essentially all of deep learning is this one algorithm applied to a [[Loss Function]], with gradients supplied by [[Backpropagation]]. Global-optimum guarantees require [[Convexity]] — which neural network losses don't have, yet GD works anyway (partly an open question).

## Related

- Prerequisite: [[Gradient]]
- Step size controlled by [[Learning Rate]]
- Gradients computed by [[Backpropagation]]
- Guarantees only under [[Convexity]]
- Second-order alternative: [[Newton's Method]]

---
Part of [[Deep Learning MOC]] and [[Mathematics MOC]] · Source: [[Deep Learning - Goodfellow (2016)]]
