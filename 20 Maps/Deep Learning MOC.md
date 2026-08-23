---
type: moc
topics: [deep-learning]
created: 2026-08-22
---

# Deep Learning MOC

> Parent: [[Machine Learning MOC]] · Math prerequisites live in [[Mathematics MOC]]

## The core loop

Everything in DL is variations on: **forward pass → loss → backward pass → update**.

- [[Neural Network]] — the model
- [[Loss Function]] — the objective
- [[Backpropagation]] — computing gradients
- [[Gradient Descent]] — using gradients to learn

## Building blocks

- [[Activation Function]] — the source of non-linearity
- [[Weight Initialization]]
- [[Regularization (ML)]] — [[Dropout]], weight decay

## Architectures

- [[Convolutional Neural Network]] — for spatial data
- [[Recurrent Neural Network]] — for sequences (mostly superseded)
- [[Transformer]] — the current default → [[Attention Mechanism]]

## Training in practice

- [[Learning Rate]] — the most important hyperparameter
- [[Batch Size]]
- [[Adam Optimizer]]

## Currently learning

- The core loop, via [[Deep Learning - Goodfellow (2016)]] #todo

## Open questions

- Why do overparameterized networks generalize at all?

## Auto-collected (unfiled)

```dataview
LIST FROM "10 Notes"
WHERE contains(topics, "deep-learning")
  AND !contains(file.outlinks, this.file.link)
```
