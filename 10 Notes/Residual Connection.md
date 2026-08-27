---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [skip connection, residual stream, shortcut connection]
---

# Residual Connection

$$y = x + F(x)$$

Each block adds its computation to an identity path instead of replacing it ([[Deep Residual Learning for Image Recognition (2015)|He 2015]]). The most consequential architectural idea of the deep-learning era after the CNN — and the backbone of every [[Transformer]] block (one skip around attention, one around the FFN).

## Why it works — the identity math

- **Diagnosis (ResNet):** deeper *plain* networks get worse **training** error — an optimization failure, not overfitting. A layer that should do nothing must learn the identity from scratch; with a skip it only needs $F \to 0$
- **Signal propagation** ([[Identity Mappings in Deep Residual Networks (2016)|He 2016]]): with pure identity skips, the forward signal between any two depths is **additive**, $x_L = x_l + \sum_{i=l}^{L-1} F(x_i)$, and the gradient carries an **unattenuated identity term**:
$$\frac{\partial \mathcal{L}}{\partial x_l} = \frac{\partial \mathcal{L}}{\partial x_L}\Big(1 + \frac{\partial}{\partial x_l}\sum_i F(x_i)\Big)$$
— no multiplicative chain along the skip path → no vanishing at any depth. Ablations: *anything* placed on the skip path (scaling, gating, dropout) hurts; **identity is optimal**
- **Results:** 152 layers trained cleanly, 3.57% top-5 ImageNet (ILSVRC-2015 sweep), 1001-layer CIFAR nets; the transformer inherited the pattern wholesale

## The residual-stream view (transformers)

Because blocks *add into* a shared stream, the transformer is best read as a **communication bus**: [[Embedding]] writes the initial state; each [[Attention Mechanism]] and [[Feedforward Network]] block reads the stream, computes an edit, and adds it back; [[Unembedding]] reads the final state. This framing underlies modern interpretability — FFN memories summing vocabulary votes ([[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|Geva 2021]]), the logit lens (apply $W_U$ mid-stream), and knowledge editing ([[ROME]]/[[MEMIT]]).

## What residuals provably prevent — and cost

- **Rank collapse:** pure attention stacks converge to rank-1 (all tokens identical) **doubly exponentially with depth**; skip connections (with the FFN) provably stop the collapse ([[Attention is Not All You Need - Rank Collapse (2021)|Dong 2021]]) — the residual is a *diversity preserver*, not just a gradient highway
- **Interaction with normalization:** where [[Normalization]] sits relative to the skip is the whole pre/post-LN story; pre-LN keeps the skip pure (stable) but lets stream variance grow with depth until deep blocks approach identity — the curse of depth ([[The Curse of Depth in LLMs (2025)|Sun 2025]]); DeepNorm re-weights the skip ($\alpha x + F$) to train 1,000 layers ([[DeepNet - Scaling Transformers to 1000 Layers (2022)|Wang 2022]])
- **Theory bonus:** spline analysis of regularized training independently justifies skip connections as natural components ([[The Role of Neural Network Activation Functions (2019)|Parhi & Nowak 2019]])

## Related

- Glue of the [[Transformer]] block, jointly with [[Normalization]]
- Enables the interpretability stack: [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)]], [[Unembedding|logit lens]], [[ROME]]
- Prevents the collapse identified in [[Attention is Not All You Need - Rank Collapse (2021)]]

## Sources

- [[Deep Residual Learning for Image Recognition (2015)]] — origin, ImageNet results
- [[Identity Mappings in Deep Residual Networks (2016)]] — the identity math
- [[Attention is Not All You Need - Rank Collapse (2021)]] — collapse prevention
- [[DeepNet - Scaling Transformers to 1000 Layers (2022)]], [[The Curse of Depth in LLMs (2025)]] — the skip×norm interaction
- [[The Role of Neural Network Activation Functions (2019)]] — spline justification

---
Part of the [[Transformer]] cluster
