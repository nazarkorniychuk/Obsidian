---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [Backprop, reverse-mode autodiff, reverse-mode automatic differentiation]
---

# Backpropagation

The algorithm that computes the gradient of the loss with respect to **every parameter** of a [[Neural Network]] efficiently: the chain rule applied *backwards* through the computation graph, reusing intermediate results ([[Learning Representations by Back-propagating Errors (1986)|Rumelhart, Hinton & Williams 1986]]). It computes gradients only — [[Gradient Descent]] decides what to do with them.

## The mechanism

Forward pass builds a composition $L = \ell(f_N(f_{N-1}(\dots f_1(x))))$. Define the **error signal** at layer $k$: $\delta_k = \partial L / \partial z_k$ (gradient w.r.t. that layer's output). Backprop is one recursion, from the loss down:

$$\delta_{k-1} = \delta_k \cdot \frac{\partial z_k}{\partial z_{k-1}}, \qquad \frac{\partial L}{\partial W_k} = \delta_k \cdot \frac{\partial z_k}{\partial W_k}$$

For a dense layer $z_k = \sigma(W_k z_{k-1})$: $\delta_{k-1} = W_k^\top(\delta_k \odot \sigma'(\cdot))$ — each layer receives the upstream error, multiplies by its **local** derivative, hands the product down.

**The efficiency theorem (why this is *the* algorithm):** reverse-mode differentiation computes the gradient w.r.t. *all $P$ parameters* at a cost of **~2–3× one forward pass, independent of $P$**. Alternatives don't scale: finite differences and forward-mode both cost $O(P)$ forward passes — for a 7B-parameter model, billions of times more. The catch: reverse mode must **store the forward activations** to compute local derivatives — memory $O(\text{depth} \times \text{activations})$, which is why training needs far more memory than inference.

## The four practical consequences

1. **Vanishing/exploding gradients:** $\delta$ traverses a *product* of layer Jacobians; if their singular values sit below/above 1, the signal shrinks/blows up geometrically with depth. The entire modern stack is engineered against this — non-saturating [[Activation Function]]s (derivative ≈ 1), [[Residual Connection]]s (an additive identity term in the gradient, no product along the skip path), [[Layer Normalization]] (re-scaling between products), variance-preserving [[Weight Initialization]] (Jacobian singular values ≈ 1 at start)
2. **Activation memory & recomputation:** storing activations dominates training memory at long sequence lengths → **gradient checkpointing**: store a subset, recompute the rest during the backward pass — trading FLOPs for memory. [[Flash Attention]]'s backward pass is exactly this trade, done per-block in SRAM
3. **Differentiability as a design constraint:** every component must pass gradients — the reason discrete choices are hard to train (the [[Mixture of Experts]] top-k non-differentiability problem and its whole balancing lineage exist because of this), and why ReLU's kink is fine (a.e. differentiable) but hard step functions are not
4. **What gets learned:** hidden units acquire task-relevant internal representations not present in input or output — the original paper's headline, and the property that separates deep learning from the perceptron era ([[Learning Representations by Back-propagating Errors (1986)]])

## History & the biology caveat

The math (reverse-mode AD) predates the fame: Linnainmaa 1970, Werbos 1974 for neural nets; Rumelhart-Hinton-Williams demonstrated representation learning and canonized it. **The brain almost certainly doesn't implement it** — backprop needs the backward pass to use the *transpose of the forward weights* (weight symmetry), biologically implausible; yet networks learn surprisingly well with *fixed random* feedback weights (feedback alignment, Lillicrap 2016) — backprop is sufficient, not necessary, for gradient-guided learning.

## Related

- Supplies gradients to [[Gradient Descent]] / [[Adam Optimizer]]
- Its failure modes shaped [[Activation Function]], [[Residual Connection]], [[Layer Normalization]], [[Weight Initialization]]
- Cost structure drives [[Flash Attention]]-style recomputation and training-memory engineering
- Linearized view of its dynamics = [[Neural Tangent Kernel]]

## Sources

- [[Learning Representations by Back-propagating Errors (1986)]] — the algorithm + representation learning
- [[Identity Mappings in Deep Residual Networks (2016)]] — the gradient identity term
- [[On Layer Normalization in the Transformer Architecture (2020)]], [[The Curse of Depth in LLMs (2025)]] — gradient flow at transformer depth

---
Part of the foundations cluster; feeds the [[Transformer]] cluster.
