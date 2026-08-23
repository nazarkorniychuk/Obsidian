---
type: concept
topics: [deep-learning, machine-learning]
status: seedling
created: 2026-08-22
aliases: [Cost Function, Objective Function]
---

# Loss Function

A function $L(\hat{y}, y)$ that assigns a number to how wrong a prediction is. Training = minimizing average loss over the dataset. The loss *defines* what the model is trying to do — change the loss, change the task.

## Common losses

- **Mean squared error** — regression: $L = \frac{1}{n}\sum (\hat{y}_i - y_i)^2$
- **Cross-entropy** — classification

## Why it matters

Most losses are not arbitrary — they fall out of [[Maximum Likelihood Estimation]] under an assumed [[Probability Distribution]] (MSE ⇔ Gaussian noise, cross-entropy ⇔ categorical).

## Related

- Minimized by [[Gradient Descent]]
- Gradients w.r.t. weights computed by [[Backpropagation]]
- Derived from [[Maximum Likelihood Estimation]]
- Poor generalization despite low loss → [[Overfitting]]

---
Part of [[Deep Learning MOC]] · Source: [[Deep Learning - Goodfellow (2016)]]
