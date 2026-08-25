---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [BatchNorm, batch normalization, normalization layers]
---

# Normalization

The family of layers that re-standardize activations so [[Gradient Descent]] faces a smoother landscape. Deep-dive on the transformer member: [[Layer Normalization]]. This note maps the family and answers the placement question precisely.

## Why normalize — the corrected mechanism

BatchNorm's stated reason ("internal covariate shift" — layer-input distributions drifting) was **experimentally refuted**: injecting distribution shift after BN leaves its benefits intact. The real mechanism: normalization makes the **loss landscape significantly smoother** (better Lipschitzness of loss and gradients) → gradients are more predictive → larger learning rates are safe → faster training ([[How Does Batch Normalization Help Optimization (2018)|Santurkar 2018]]). Same conditioning story as [[Neural Tangent Kernel]]-based explanations elsewhere in this vault.

## ⚡ The family — what dimension you normalize over

| Method | Statistics over | Formula core | Domain | Fatal constraint |
| --- | --- | --- | --- | --- |
| **BatchNorm** | the batch, per channel | $(x-\mu_B)/\sigma_B \cdot \gamma + \beta$ | ConvNets | needs large batches; train≠test stats; couples examples → unusable for autoregressive decode ([[Batch Normalization (2015)\|Ioffe & Szegedy 2015]]) |
| GroupNorm | channel groups, per sample | same, grouped | vision, small-batch | vision-specific |
| **LayerNorm** | the feature dim, per token | $(x-\mu)/\sigma \cdot \gamma + \beta$ | transformers | two reductions; see [[Layer Normalization]] |
| **RMSNorm** | feature dim, scale only | $x / \text{RMS}(x) \cdot \gamma$ | **LLM default** | none — that's the point |
| DyT | none (no statistics) | $\tanh(\alpha x)$ | research | [[Transformers without Normalization (2025)\|the "was it ever needed" result]] |

- **BatchNorm results:** 14× fewer training steps, 4.82% top-5 ImageNet — the technique that made deep ConvNets routine; its batch-coupling is exactly why transformers *can't* use it (each token's statistics would depend on batch-mates — broken at decode, batch 1)
- **RMSNorm** ([[Root Mean Square Layer Normalization (2019)|Zhang & Sennrich 2019]]): drop LayerNorm's mean-subtraction and bias — hypothesis: re-centering is dispensable. **Comparable quality, 7–64% runtime reduction**; adopted by LLaMA/Mistral/Qwen/DeepSeek/Gemma. Later formalized: in pre-norm transformers the stream's mean component is redundant — Pre-LN ≡ Pre-RMSNorm up to conversion

## Placement — where the norm sits in the block (the exact answer)

Using RMSNorm as the modern default, the three placements are:

**Pre-norm (LLaMA-family standard):** the norm is applied to the **sublayer input, inside the residual branch — before attention and before the FFN — never on the residual path itself:**
$$h' = h + \text{Attn}\big(\text{RMSNorm}(h)\big) \qquad h'' = h' + \text{FFN}\big(\text{RMSNorm}(h')\big)$$
plus one **final RMSNorm** after the last block, before the [[Unembedding]]. So per block: 2 norms (LLaMA), the skip connection carries the raw un-normalized stream. Why it won: gradients well-behaved at init, warmup removable ([[On Layer Normalization in the Transformer Architecture (2020)|Xiong 2020]]); cost: stream variance grows with depth → deep layers drift toward identity ([[The Curse of Depth in LLMs (2025)|curse of depth]])

**Post-norm (original Transformer):** norm applied **after the residual addition**, on the main path:
$$h' = \text{LN}\big(h + \text{Attn}(h)\big)$$
Better per-layer effectiveness, but unstable at depth without warmup/DeepNorm ([[DeepNet - Scaling Transformers to 1000 Layers (2022)|Wang 2022]])

**Peri-norm (Gemma-2-style sandwich):** normalize **both the input and the output of each sublayer**, keeping the skip clean:
$$h' = h + \text{RMSNorm}_{out}\big(\text{Attn}(\text{RMSNorm}_{in}(h))\big)$$
— bounds each branch's contribution before it enters the stream, giving balanced variance growth and steadier gradients up to 3.2B in controlled comparisons ([[Peri-LN (2025)|Kim 2025]]); 4 norms per block. Related placements: **QK-norm** (normalize queries/keys inside attention — logit-explosion control) composes with any of the above

**The invariant across all modern variants: nothing is ever placed *on* the skip path** — the identity gradient highway ([[Residual Connection]]) stays untouched; only where the branch gets normalized differs.

## Related

- Transformer member deep-dive: [[Layer Normalization]] (pre/post-LN theory, curse of depth, DyT)
- Preserves the [[Residual Connection]] highway; stabilizes [[Backpropagation]]; complements [[Weight Initialization]] (init handles step 0, normalization handles the rest)
- The "smoother landscape" mechanism is the same conditioning story as [[Optimizers]]/[[Neural Tangent Kernel]]

## Sources

- [[Batch Normalization (2015)]] — the founding member + its constraints
- [[How Does Batch Normalization Help Optimization (2018)]] — the corrected mechanism
- [[Root Mean Square Layer Normalization (2019)]] — the LLM standard
- [[On Layer Normalization in the Transformer Architecture (2020)]], [[DeepNet - Scaling Transformers to 1000 Layers (2022)]], [[The Curse of Depth in LLMs (2025)]] — the placement theory
- [[Peri-LN (2025)]] — the placement frontier
- [[Transformers without Normalization (2025)]] — the null hypothesis

---
Part of the foundations cluster.
