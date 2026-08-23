---
type: source
source-type: paper
authors: [William Fedus, Barret Zoph, Noam Shazeer]
year: 2021
url: https://consensus.app/papers/details/ede62b54fd22506aa732c87ed6fe413a/?utm_source=claude_desktop
doi: 10.48550/arxiv.2101.03961
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-23
aliases: [Switch Transformer paper, Fedus 2021]
---

# Switch Transformers (2021)

*Full title: "Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity"* (~4,600 citations).

## Summary

Makes [[Mixture of Experts]] practical at scale by radical simplification: route each token to **one** expert (top-1) instead of top-k, cutting routing compute and communication. Adds selective-precision training (bf16-stable), expert dropout, and an auxiliary load-balancing loss.

## Key results

- **7× pre-training speedup** over T5-Base at the **same FLOPs per token** (parameters ↑, compute flat)
- First **trillion-parameter** (1.6T, 2048 experts) language model trained; **4× speedup over T5-XXL**
- Gains persist across 101 languages (multilingual mT5 comparison)
- Sparse models are effective distillation teachers: compress back into dense students keeping ~30% of the gain

## Impact

Template for the modern MoE wave: GLaM, Mixtral 8×7B (47B total / 13B active), DeepSeek-V2/V3 (671B total / 37B active), Qwen-MoE. Established that **parameter count and FLOPs are separable axes** of scaling.

## Concepts extracted

- [[Mixture of Experts]] — the anchor result
- [[Feedforward Network]] — MoE = routed FFN section
