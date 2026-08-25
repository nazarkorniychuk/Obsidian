---
type: source
source-type: paper
authors: [Arthur Jacot, Franck Gabriel, Clément Hongler]
year: 2018
url: https://consensus.app/papers/details/bd116b003c175d52be82436ee8bee6b0/?utm_source=claude_desktop
doi: 10.48550/arxiv.1806.07572
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [NTK paper, Jacot 2018]
---

# Neural Tangent Kernel - Jacot (2018)

*Full title: "Neural Tangent Kernel: Convergence and Generalization in Neural Networks"* (NeurIPS 2018, ~4,200 citations).

## Summary

The evolution of a network's *function* during gradient descent follows kernel gradient descent with respect to a specific kernel — the **NTK**, $\Theta(x, x') = \nabla_\theta f(x)^\top \nabla_\theta f(x')$. The discovery: **in the infinite-width limit the NTK converges to a deterministic kernel and stays constant during training** — training the network becomes exactly solvable kernel regression.

## Key results

- Training dynamics in function space become **linear**: for square loss, the function follows a linear ODE; convergence is fastest along the kernel's top principal components (a theoretical motivation for early stopping)
- Convergence guarantees tied to positive-definiteness of the limiting kernel

## Impact & limits

Founded the theoretical program this vault's optimization results live in: [[The Devil is in the Condition Numbers (2026)|GLU conditioning]], convergence analyses, condition-number reasoning. Its limit is equally important: the constant-kernel ("lazy") regime means **features don't move** — real networks at real scale do learn features, and [[Tensor Programs V - muTransfer (2022)|μP]] is precisely the parameterization that keeps feature learning alive at infinite width. NTK = the solvable idealization; μP = the corrective.

## Concepts extracted

- [[Neural Tangent Kernel]] — the founding theorem
