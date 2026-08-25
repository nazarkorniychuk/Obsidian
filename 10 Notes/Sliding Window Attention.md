---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [SWA, local attention, windowed attention]
---

# Sliding Window Attention

Local variant of the [[Attention Mechanism]]: each token attends only to its $w$ nearest neighbors → cost drops $O(T^2) \to O(T \cdot w)$, and the [[KV Cache]] becomes a **bounded rolling buffer** of $w$ tokens instead of growing with context.

## Mechanism & reach

- Attention mask restricted to $|i - j| \le w$ (causal: $0 \le i - j \le w$)
- **Receptive field grows with depth:** layer $\ell$'s output at position $i$ depends on inputs up to $\ell \cdot w$ away — like stacked convolutions. Information beyond the window isn't unreachable, it must **hop layer-by-layer** (32 layers × $w{=}4096$ → theoretical reach 131k; practical reach much shorter since each hop is lossy)
- **Longformer additions** ([[Longformer (2020)|Beltagy 2020]]): dilated windows (gaps widen reach per layer) and **global tokens** — a few designated positions ([CLS], question tokens) that attend everywhere and are attended by all, restoring $O(1)$-hop global communication at $O(T)$ extra cost

## Results

- SOTA character-level LM (text8, enwik8) at publication; pretrained Longformer **beat RoBERTa on every long-document task tested** (WikiHop, TriviaQA SOTA) at 4096 vs 512 context ([[Longformer (2020)]])
- **The humbling replication** ([[Simple Local Attentions Remain Competitive (2021)|Xiong 2021]]): under identical pretrain-finetune protocol, *no* sophisticated efficient attention beat the plain local window — and even the window **overlap is unnecessary**: disjoint (blocked) local attention matched Longformer QA at **half the pretraining compute**
- Production: [[Language Models are Few-Shot Learners - GPT-3 (2020)|GPT-3]] alternated dense and locally-banded sparse layers; **Mistral-7B** shipped $w{=}4096$ SWA with a rolling-buffer cache; **Gemma-2/3** interleave local:global layers (e.g. 5:1 in Gemma-3) — the surviving pattern is the **hybrid**, not pure SWA

## The eviction trap — attention sinks

Naive "keep the last $w$ tokens" **collapses** on long streams: perplexity explodes the moment the *first* tokens leave the cache, because softmax heads park excess attention on initial tokens (**attention sinks**) regardless of content. Fix costs ~4 tokens: keep the sink tokens + rolling window → stable on 4M+ token streams, 22.2× faster than recompute-window baselines ([[StreamingLLM - Attention Sinks (2023)|Xiao 2023]]). Any SWA deployment that evicts must reserve the sinks.

## Where SWA sits in the design space

- **vs full attention:** exact local recall, no global reach — complements a global mechanism rather than replacing it
- **vs [[Linear Attention]]:** SWA keeps a *perfect* recent memory of size $w$; linear attention keeps a *lossy* summary of everything. The recall-throughput frontier is traversed by **combining them** ([[BASED - Recall-Throughput Tradeoff (2024)|Arora 2024]]); the strongest hybrids interleave SSM layers with SWA layers — Samba: trained at 4K, perfect passkey recall at 256K, 3.73× throughput vs GQA transformers at 128K ([[Samba (2024)|Ren 2024]])
- **vs rank collapse:** local masks provably *slow* attention's token-uniformity collapse (follow-up to [[Attention is Not All You Need - Rank Collapse (2021)|Dong 2021]]) — a small theoretical bonus
- **ALiBi connection:** [[ALiBi (2021)|ALiBi]]'s linear distance penalty acts as an *implicit soft* sliding window — its famed length extrapolation is largely window behavior in disguise

## Related

- Variant of the [[Attention Mechanism]]; bounds the [[KV Cache]]
- Hybrid partners: [[Linear Attention]] / [[Mamba (2023)]]-style SSMs (global gist) + SWA (local precision)
- Eviction interplay: [[StreamingLLM - Attention Sinks (2023)]]

## Sources

- [[Longformer (2020)]] — the canonical formulation + results
- [[Language Models are Few-Shot Learners - GPT-3 (2020)]] — production banded-sparse
- [[Simple Local Attentions Remain Competitive (2021)]] — the baseline-wins result
- [[StreamingLLM - Attention Sinks (2023)]] — the eviction trap
- [[Samba (2024)]], [[BASED - Recall-Throughput Tradeoff (2024)]] — SWA as hybrid ingredient
- [[ALiBi (2021)]] — implicit-window connection

---
Part of the [[Transformer]] cluster
