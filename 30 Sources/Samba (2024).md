---
type: source
source-type: paper
authors: [Liliang Ren, et al. (Microsoft)]
year: 2024
url: https://consensus.app/papers/details/8537cc6b215d5c85aa69d6d9b30370a2/?utm_source=claude_desktop
doi: 10.48550/arxiv.2406.07522
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Ren 2024, hybrid SSM-SWA]
---

# Samba (2024)

*Full title: "Samba: Simple Hybrid State Space Models for Efficient Unlimited Context Language Modeling."*

## Summary

The hybrid thesis, executed simply: interleave **Mamba layers** (compress history into a recurrent state — the gist) with **[[Sliding Window Attention]] layers** (precise recall of recent tokens) layer-by-layer. Each mechanism covers the other's weakness.

## Key results

- 3.8B params / 3.2T tokens: **beats state-of-the-art models across benchmarks** at its scale
- Trained at 4K length: **zero-shot perplexity improvements up to 1M tokens**; fine-tuned at 4K → **perfect passkey recall at 256K**, beating full-attention models on retrieval extrapolation
- **3.73× throughput** vs GQA-transformers at 128K prompts; 3.64× speedup generating 64K tokens

## Impact

Blueprint for the production hybrid wave (Phi-family adoption, Jamba-style dense+SSM mixes). The practical resolution of the linear-vs-full-attention fight: don't choose — compose.

## Concepts extracted

- [[Linear Attention]] — the hybrid resolution; [[Sliding Window Attention]]
