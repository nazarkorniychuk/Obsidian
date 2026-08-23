---
type: moc
topics: [linear-algebra, probability, calculus, optimization]
created: 2026-08-22
---

# Mathematics MOC

> Parent: [[Home]] · Serves as the foundation layer for [[Machine Learning MOC]]

## Linear algebra

- [[Matrix Multiplication]] — what a neural network layer *is*
- [[Eigenvalue]]

## Calculus

- [[Gradient]] — direction of steepest ascent
- [[Chain Rule]] — the engine inside [[Backpropagation]]

## Probability & statistics

- [[Probability Distribution]]
- [[Maximum Likelihood Estimation]] — where most [[Loss Function]]s come from

## Optimization

- [[Gradient Descent]]
- [[Convexity]] — when optimization has guarantees (DL: it doesn't)

## Auto-collected (unfiled)

```dataview
LIST FROM "10 Notes"
WHERE (contains(topics, "linear-algebra") OR contains(topics, "probability") OR contains(topics, "calculus") OR contains(topics, "optimization"))
  AND !contains(file.outlinks, this.file.link)
```
