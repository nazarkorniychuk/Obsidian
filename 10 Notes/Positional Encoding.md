---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [RoPE, positional embedding, position encoding, ALiBi]
---

# Positional Encoding

The [[Attention Mechanism]] is **permutation-equivariant** — $QK^\top$ scores depend only on content, so "the dog bit the man" and "the man bit the dog" are indistinguishable without injected position information. Positional encoding is that injection; its design determines whether a model can generalize beyond its training length.

## ⚡ The lineage

| Scheme | Introduced by | Mechanism | Adopters | Fate |
| --- | --- | --- | --- | --- |
| Sinusoidal (absolute) | [[Attention Is All You Need (2017)\|Vaswani 2017]] | add fixed $\sin/\cos$ waves to embeddings | original Transformer | superseded |
| Learned absolute | GPT-1 / [[BERT (2019)\|BERT]] era | trainable vector per position | BERT, GPT-1/2/3 | hard length cap; superseded |
| Relative (learned) | [[Self-Attention with Relative Position Representations (2018)\|Shaw 2018]] | embed clipped $j{-}i$ into attention | T5 (bucketed biases) | founded the relative turn |
| **RoPE** | [[RoFormer (2021)\|Su 2021]] | rotate $q,k$ by position-dependent angles | **LLaMA, Mistral, Qwen, DeepSeek, Gemma — the standard** | current default |
| ALiBi | [[ALiBi (2021)\|Press 2021]] | subtract $m\|i{-}j\|$ from logits | BLOOM, MPT | lost to RoPE; ideas recur |
| **NoPE** (none) | analyzed by [[The Impact of Positional Encoding on Length Generalization (2023)\|Kazemnejad 2023]] | causal mask alone | research | the surprise baseline |

## The absolute era

- **Sinusoidal:** $PE_{(pos, 2i)} = \sin(pos/10000^{2i/d})$, $PE_{(pos,2i+1)} = \cos(\cdot)$ — a spectrum of wavelengths from $2\pi$ to $10000 \cdot 2\pi$; chosen because relative shifts are linear functions of the encodings ([[Attention Is All You Need (2017)|Vaswani 2017]] — who found learned positions performed identically)
- **Learned absolute:** one trainable vector per index — simple, but positions beyond training length literally don't exist (BERT's hard 512 cap)
- **The relative turn** ([[Self-Attention with Relative Position Representations (2018)|Shaw 2018]]): inject learned embeddings of clipped $j - i$ into attention instead: **+1.3 BLEU** En-De over absolute; combining relative *and* absolute added nothing — relative offsets are what attention actually uses

## RoPE — the standard

Pair up dimensions and **rotate** each pair of $q$ and $k$ by angle $m\theta_i$ at position $m$, frequencies $\theta_i = b^{-2i/d}$ (base $b = 10000$). The identity that made it win:

$$(R_m q)^\top (R_n k) = q^\top R_{n-m}\,k$$

— absolute rotations, **relative scores**. Zero parameters, no runtime state, distance-decaying interactions, works with any length, compatible with linear attention ([[RoFormer (2021)|Su 2021]]). Consistently beat alternatives on long-text tasks; adopted by essentially every current LLM.

**Long-context extension** (the practical art): trained RoPE fails *abruptly* past its training length. Retrofits: **position interpolation** (rescale positions $m \to m/s$ into the trained range, brief fine-tune), **base scaling** (raise $b$: 10k → 500k → millions — the standard 128k+ recipe), NTK-aware/YaRN interpolation variants (unfetched here). **The provable ceiling** ([[RoPE Distinguishes Neither Positions Nor Tokens (2026)|Du 2026]]): as context grows, RoPE attention loses locality bias *and* score consistency (failure probability → 0.5), and raising the base trades token-distinguishing against position-distinguishing — **it cannot preserve both**; million-token contexts sit inside a provable degradation regime.

**The MLA conflict:** RoPE's rotation is position-dependent, so it doesn't commute with [[Multi-Head Latent Attention]]'s absorbed up-projections — forcing MLA's decoupled 64-dim RoPE key. The cleanest evidence that PE choice is a *hard architectural constraint*, not a detail.

## ALiBi — and what its success actually was

No embeddings at all: score$(i,j) = q_i k_j - m|i - j|$ with fixed per-head slopes (geometric across heads). **Train at 1024, extrapolate to 2048 at the perplexity of a model trained at 2048**, 11% faster/leaner training ([[ALiBi (2021)|Press 2021]]). The catch (receptive-field analysis, Chi 2022): the linear penalty is an **implicit [[Sliding Window Attention|sliding window]]** — ALiBi extrapolates perplexity while barely *using* tokens beyond the training range. Honest summary: a strong recency prior, not true long-range extrapolation.

## The NoPE surprise

Decoder-only transformers **don't strictly need** positional encoding: the **causal mask already breaks permutation symmetry** (each position sees a different-length prefix). Systematic comparison on length-generalization tasks: **NoPE beat RoPE, ALiBi, APE, and T5-relative** — all the popular schemes are *poor* at downstream length generalization, and the best explicit scheme was beaten by none at all; theoretically NoPE can represent both absolute and relative encodings, and trained models mimic T5-relative patterns ([[The Impact of Positional Encoding on Length Generalization (2023)|Kazemnejad 2023]]). Caveat: small/medium models, synthetic tasks — frontier LLMs still ship RoPE, but the result reframes PE as an *inductive-bias choice*, not a requirement.

## Hardware note

PE cost is negligible next to attention/FFN, but not zero: RoPE's element-wise rotations are memory-bound and awkwardly interleaved (split/rotate/merge per head per step) — measured at a substantial fraction of non-GEMM execution time in some inference stacks, motivating fused kernels and even PIM accelerators. Practical rule: **fuse RoPE into the attention kernel** (standard in [[Flash Attention]]-family implementations).

## Related

- Prerequisite of the [[Attention Mechanism]] (step 2 of the [[Transformer]] forward pass)
- Constrains [[Multi-Head Latent Attention]] (decoupled key); interacts with [[Sliding Window Attention]] (ALiBi ≈ implicit window) and long-context [[KV Cache]] scaling
- Future: [[YaRN]] / NTK-aware scaling deep-dive if needed

## Sources

- [[Attention Is All You Need (2017)]] — sinusoidal
- [[Self-Attention with Relative Position Representations (2018)]] — the relative turn
- [[RoFormer (2021)]] — RoPE
- [[ALiBi (2021)]] — linear biases + extrapolation caveat
- [[The Impact of Positional Encoding on Length Generalization (2023)]] — NoPE
- [[RoPE Distinguishes Neither Positions Nor Tokens (2026)]] — RoPE's provable long-context ceiling
- [[DeepSeek-V2 (2024)]] — the MLA decoupling

---
Part of the [[Transformer]] cluster
