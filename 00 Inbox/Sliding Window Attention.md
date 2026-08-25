---
type: concept
topics: [deep-learning]
status: seedling
created: 2026-08-24
aliases: [SWA, local attention, windowed attention]
---

# Sliding Window Attention

## Related

- Variant of [[Attention Mechanism]] — $O(Tw)$ local windows; receptive field $\ell \cdot w$ grows with depth
- To cover in depth: global-token augmentation, dilated windows, local:global hybrid layering (Gemma-2/3 pattern), rolling-buffer KV cache, why the dumb baseline beat the clever sparse variants ([[Simple Local Attentions Remain Competitive (2021)]])
- Sources ready: [[Longformer (2020)]], [[Language Models are Few-Shot Learners - GPT-3 (2020)|GPT-3's banded pattern]]
- Interacts with [[KV Cache]] (window bounds cache size)
