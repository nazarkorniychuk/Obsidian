---
type: concept
topics: [deep-learning]
status: growing
created: 2026-08-22
aliases: [NN, Feedforward Network, MLP, Multilayer Perceptron]
---

# Neural Network

A function built by composing simple layers: each layer multiplies its input by a weight matrix, adds a bias, and applies a non-linearity. Stacking layers lets the network approximate arbitrarily complex functions.

$$h^{(l+1)} = \sigma\left(W^{(l)} h^{(l)} + b^{(l)}\right)$$

## Intuition

Each layer re-represents its input in a new coordinate system. Early layers learn simple features, later layers compose them into abstract ones. "Deep" = many compositions.

Without the non-linearity $\sigma$ (see [[Activation Function]]), stacking layers collapses into a single [[Matrix Multiplication]] — depth would buy nothing.

## Why it matters

The universal building block. Every modern architecture ([[Convolutional Neural Network]], [[Transformer]]) is this pattern plus structural constraints.

## How it learns

1. Forward pass: compute prediction
2. Compare to target via a [[Loss Function]]
3. Compute gradients via [[Backpropagation]]
4. Update weights via [[Gradient Descent]]

## Related

- Trained by [[Gradient Descent]] using [[Backpropagation]]
- Objective defined by a [[Loss Function]]
- Non-linearity from [[Activation Function]]
- A single layer is just [[Matrix Multiplication]] + bias

---
Part of [[Deep Learning MOC]] · Source: [[Deep Learning - Goodfellow (2016)]]
