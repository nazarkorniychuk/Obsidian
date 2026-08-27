---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [LayerNorm, LN, RMSNorm, BatchNorm, Layer Normalization, batch normalization]
---

# Normalization

The layers that re-standardize activations so [[Gradient Descent]] faces a smoother landscape — and whose *placement* in the [[Transformer]] block turned out to matter more than their formulas.

## Why normalize — the corrected mechanism

BatchNorm's stated reason ("internal covariate shift" — layer-input distributions drifting during training) was **experimentally refuted**: injecting deliberate distribution shift after BN layers leaves its benefits intact. The real mechanism: normalization makes the **loss landscape significantly smoother** (better Lipschitzness of loss and gradients) → gradients are more predictive → larger learning rates are safe → faster training ([[How Does Batch Normalization Help Optimization (2018)|Santurkar 2018]]). The same conditioning story as the [[Neural Tangent Kernel]] explanations used across this vault — and a standing caution about named mechanisms that outlive their falsification.

## ⚡ The family — what dimension you normalize over

| Method | Statistics over | Formula core | Domain | Constraint |
| --- | --- | --- | --- | --- |
| **BatchNorm** | the batch, per channel | $(x-\mu_B)/\sigma_B \cdot \gamma + \beta$ | ConvNets | needs large batches; train≠test stats; couples examples → unusable for autoregressive decode ([[Batch Normalization (2015)\|Ioffe & Szegedy 2015]]) |
| GroupNorm | channel groups, per sample | same, grouped | vision, small-batch | vision-specific |
| **LayerNorm** | feature dim, per token | $\gamma \odot \frac{x-\mu(x)}{\sigma(x)} + \beta$ | transformers | two reductions per call |
| **RMSNorm** | feature dim, scale only | $\gamma \odot \frac{x}{\sqrt{\frac{1}{d}\sum_i x_i^2}}$ | **LLM default** | none — that's the point |
| DyT | none (no statistics) | $\tanh(\alpha x)$ | research | see below |

- **BatchNorm results:** 14× fewer training steps, 4.82% top-5 ImageNet — made deep ConvNets routine; its batch-coupling is exactly why transformers *can't* use it (a token's statistics would depend on batch-mates — broken at decode, batch 1). LayerNorm/RMSNorm normalize **within one token** — batch-independent, autoregression-safe
- **RMSNorm** ([[Root Mean Square Layer Normalization (2019)|Zhang & Sennrich 2019]]): drop LayerNorm's mean-subtraction and bias ("re-centering is dispensable"). **Comparable quality, 7–64% runtime reduction**; adopted by LLaMA/Mistral/Qwen/DeepSeek/Gemma. Later formalized: in pre-norm transformers the residual stream's mean component is redundant — Pre-LN ≡ Pre-RMSNorm up to conversion, retroactively proving the hypothesis for exactly the architecture that adopted it

## Placement — where the norm sits in the block

The central design question. Three placements, with RMSNorm as the modern default; **the invariant across all of them: nothing is ever placed *on* the skip path** — the [[Residual Connection]]'s identity gradient highway stays untouched.

**Post-norm (original Transformer):** after the residual addition, on the main path — $h' = \text{LN}(h + \text{Attn}(h))$. Mean-field analysis at initialization: gradients near the output are **large** → training diverges at real learning rates → the mysterious learning-rate **warmup stage** exists solely to survive this ([[On Layer Normalization in the Transformer Architecture (2020)|Xiong 2020]]). Better per-layer effectiveness, unstable at depth. Rescued by **DeepNorm** ($\text{LN}(\alpha x + f(x))$ with derived constants): **1,000-layer transformers** train cleanly; a 200-layer/3.2B model beat a 48-layer/12B SOTA by 5 BLEU on massively multilingual MT ([[DeepNet - Scaling Transformers to 1000 Layers (2022)|Wang 2022]])

**Pre-norm (GPT-2 → LLaMA standard):** the norm is applied to the **sublayer input, inside the residual branch — before attention and before the FFN, never on the skip:**
$$h' = h + \text{Attn}\big(\text{RMSNorm}(h)\big) \qquad h'' = h' + \text{FFN}\big(\text{RMSNorm}(h')\big)$$
plus one **final RMSNorm** after the last block, before the [[Unembedding]] (2 norms per block). Why it won: gradients well-behaved at init → **warmup removable**, deep training stable ([[On Layer Normalization in the Transformer Architecture (2020)|Xiong 2020]]). **The hidden cost — curse of depth:** the residual stream's variance grows with depth, so each deep block's normalized contribution shrinks — deep-layer derivatives approach identity and **nearly half the layers of Llama/Mistral/DeepSeek/Qwen-class models under-contribute**; fixable by scaling LN output by $1/\sqrt{\ell}$ (LayerNorm Scaling, beats prior schemes on 130M–7B pre-training) ([[The Curse of Depth in LLMs (2025)|Sun 2025]])

**Peri-norm (Gemma-2-style sandwich):** normalize **both the input and the output of each sublayer**, skip still clean:
$$h' = h + \text{RMSNorm}_{out}\big(\text{Attn}(\text{RMSNorm}_{in}(h))\big)$$
— bounds each branch's contribution before it enters the stream, attacking pre-norm's variance growth at the source: more balanced variance, steadier gradients, better convergence stability up to 3.2B in controlled comparisons ([[Peri-LN (2025)|Kim 2025]]); 4 norms per block. Composable extra: **QK-norm** (normalize queries/keys inside attention — attention-logit-explosion control)

## What normalization does beyond stability

- **Expressivity role (geometric view):** LN = projection onto the hyperplane ⊥ $[1,\dots,1]$ + rescale to norm $\sqrt d$ — the projection lets attention form queries that attend uniformly; the rescaling keeps every key *selectable* (no "un-attendable" tokens)
- **Rank collapse interaction:** with suitable value matrices, attention+LN can sustain equilibria of *any* rank — normalization is part of why real transformers escape pure attention's rank-1 collapse ([[Attention is Not All You Need - Rank Collapse (2021)|Dong 2021]] and follow-ups)
- **Maybe it's just a squash:** trained LNs implement tanh-like S-curves; replacing all normalization with $\tanh(\alpha x)$ (**DyT** — a learned scalar, no statistics computed) **matches or beats normalized transformers** across vision and language ([[Transformers without Normalization (2025)|Zhu 2025]]) — the per-token statistics may never have been the essential part

## Hardware note

Normalization is a reduction (mean/variance across $d$) — cheap in FLOPs but a kernel-fusion nuisance (a sync point between GEMMs). RMSNorm halves the reductions; DyT removes them entirely — part of each variant's appeal.

## Related

- Interacts with [[Residual Connection]] (placement = the pre/post question), [[Backpropagation]] (gradient flow), [[Weight Initialization]] (init handles step 0, normalization the rest), [[Adam Optimizer]] (warmup)
- Softmax-numerics cousins: router z-loss ([[ST-MoE (2022)]]), logit capping ([[Unembedding]])
- Mechanism kinship: [[Optimizers]] / [[Neural Tangent Kernel]] conditioning

## Sources

- [[Batch Normalization (2015)]] — the founding member + constraints
- [[How Does Batch Normalization Help Optimization (2018)]] — the corrected mechanism
- [[Root Mean Square Layer Normalization (2019)]] — the LLM standard
- [[On Layer Normalization in the Transformer Architecture (2020)]] — pre vs post, warmup
- [[DeepNet - Scaling Transformers to 1000 Layers (2022)]] — post-norm at 1,000 layers
- [[The Curse of Depth in LLMs (2025)]] — pre-norm's depth pathology
- [[Peri-LN (2025)]] — the placement frontier
- [[Transformers without Normalization (2025)]] — DyT
- [[Attention is Not All You Need - Rank Collapse (2021)]] — normalization in collapse dynamics

---
Part of the foundations cluster; glue of the [[Transformer]] block.
