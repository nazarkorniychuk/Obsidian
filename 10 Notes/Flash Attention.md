---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [FlashAttention, FA1, FA2]
---

# Flash Attention

IO-aware **exact** computation of the [[Attention Mechanism]] — an implementation, not an architecture: same math, same result, restructured around the GPU memory hierarchy ([[FlashAttention (2022)|Dao 2022]]).

## The problem — attention is IO-bound, not FLOP-bound

- GPU memory hierarchy (A100): **HBM** 40–80 GB at ~1.5–2 TB/s vs **on-chip SRAM** ~192 KB/SM at ~19 TB/s — an order of magnitude bandwidth gap
- Naive attention makes three full passes through HBM: write $S = QK^\top$ ($T \times T$), read it to compute $P = \text{softmax}(S)$, read $P$ to compute $O = PV$ → $O(T^2)$ HBM traffic and $O(T^2)$ memory; at $T{=}8192$, $S$ alone is 128 MB *per head*
- The FLOPs are cheap; **moving $S$ and $P$ through HBM is the bottleneck**

## The algorithm

1. **Tiling:** split $Q, K, V$ into blocks sized to fit SRAM; compute attention block-by-block without ever materializing the full $T \times T$ matrix
2. **Online softmax:** softmax normally needs the whole row before normalizing. Instead keep running statistics per row — max $m$ and sum $\ell$ — and **rescale previous partial outputs** as new blocks arrive:
$$m^{new} = \max(m, \tilde m), \qquad \ell^{new} = e^{m - m^{new}}\ell + e^{\tilde m - m^{new}}\tilde\ell$$
   with the partial output $O$ rescaled by the same factors. Exact, not approximate
3. **Backward by recomputation:** store only $(O, m, \ell)$ — $O(T)$ — and **recompute** $S, P$ block-wise during backprop. More FLOPs, far fewer bytes; net faster because the operation is IO-bound
4. **IO complexity:** $\Theta(T^2 d^2 / M)$ HBM accesses (SRAM size $M$) vs standard $\Theta(T^2 + Td)$ — proven optimal over a range of $M$

## Results

- **15%** end-to-end over the MLPerf 1.1 BERT-large record; **3×** GPT-2 (seq 1K); **2.4×** Long-Range Arena ([[FlashAttention (2022)|Dao 2022]])
- Longer affordable context → better models: −0.7 GPT-2 perplexity, +6.4 pts long-document classification; **first transformer above chance on Path-X** (16K, 61.4%)
- **FA2** ([[FlashAttention-2 (2023)|Dao 2023]]): FA1 reached only 25–40% of peak — bottleneck moved to **work partitioning**. Fixes: fewer non-matmul FLOPs, parallelize across the *sequence* dimension (not just batch×heads — critical for long sequences with few heads), split work between warps to cut shared-memory traffic. **~2× over FA1, 50–73% of A100 peak, 225 TFLOPs/s/GPU (72% MFU)** end-to-end
- Block-sparse FA: faster than every *approximate* method compared — the exact algorithm beat the approximations at their own game

## Adoption & consequences

- **Universal:** PyTorch `scaled_dot_product_attention`, every LLM training/serving stack; FA3 continues the line for Hopper GPUs (hardware-specific: TMA, fp8 — not yet sourced in this vault)
- **Strategic consequence:** by deleting the wall-clock advantage of approximate attention, FA is a main cause of the efficient-attention extinction (see [[Attention Mechanism]] → failure story)
- The IO-aware recipe generalized beyond softmax attention: FlashLinearAttention ([[Gated Linear Attention (2023)|Yang 2023]]), FlashMLA, Mamba's fused scan ([[Mamba (2023)|Gu & Dao 2023]]) — "count bytes, not FLOPs" became the default kernel-design doctrine
- **Limits:** FA optimizes training/prefill (the $T\times T$ problem); decode is bottlenecked by [[KV Cache]] loads instead — a different problem with different solutions ([[Grouped-Query Attention]], [[Multi-Head Latent Attention]])

## Related

- Implementation layer of the [[Attention Mechanism]]
- Same doctrine applied elsewhere: [[Gated Linear Attention (2023)]], [[Mamba (2023)]]
- Decode-side counterpart: [[KV Cache]]

## Sources

- [[FlashAttention (2022)]] — algorithm, IO analysis, results
- [[FlashAttention-2 (2023)]] — work partitioning, peak-utilization numbers
- [[On The Computational Complexity of Self-Attention (2022)]] — why $O(T^2)$ FLOPs can't be beaten exactly (FA reduces bytes, not FLOPs)

---
Part of the [[Transformer]] cluster
