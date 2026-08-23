---
type: concept
topics: [deep-learning, calculus]
status: growing
created: 2026-08-22
aliases: [Backprop, Reverse-mode Autodiff]
---

# Backpropagation

An efficient algorithm for computing the gradient of the [[Loss Function]] with respect to *every* weight in a [[Neural Network]], by applying the [[Chain Rule]] backwards through the computation graph.

## Intuition

The forward pass builds a composition of functions. The chain rule says the derivative of a composition is a product of local derivatives. Backprop computes these products **from the loss backwards**, reusing intermediate results — so the full gradient costs about the same as one forward pass, instead of one pass *per weight*.

> [!note]- The core equation
> For a layer with output $z = f(w, a)$ feeding into loss $L$:
> $$\frac{\partial L}{\partial w} = \frac{\partial L}{\partial z} \cdot \frac{\partial z}{\partial w}$$
> The term $\frac{\partial L}{\partial z}$ is received from the layer above; you compute your local derivative and pass the product down.

## Why it matters

Backprop is *the* enabler of deep learning: without it, training networks with millions of parameters would be computationally impossible. It is not a learning rule — it only computes gradients; [[Gradient Descent]] does the learning.

## Common confusion

Backprop ≠ training. Backprop answers "what is the gradient?"; [[Gradient Descent]] answers "what do we do with it?"

## Related

- Is the [[Chain Rule]] applied to computation graphs
- Supplies gradients to [[Gradient Descent]]
- Operates on a [[Neural Network]]'s forward computation
- Failure mode in deep/recurrent nets: vanishing gradients → motivates [[Activation Function]] choice and architectures like the [[Transformer]]

---
Part of [[Deep Learning MOC]] · Source: [[Deep Learning - Goodfellow (2016)]]
