---
type: source
source-type: paper
authors: [Guangxuan Xiao, Yuandong Tian, Beidi Chen, Song Han, Mike Lewis]
year: 2023
url: https://consensus.app/papers/details/bced5168c14258b996db0d2e02f1f0ee/?utm_source=claude_desktop
doi: 10.48550/arxiv.2309.17453
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [attention sinks paper, Xiao 2023]
---

# StreamingLLM - Attention Sinks (2023)

*Full title: "Efficient Streaming Language Models with Attention Sinks"* (~2,300 citations).

## Summary

Discovered the **attention sink**: LLMs dump large attention mass onto the *first* tokens regardless of semantic content — because softmax must sum to 1, heads with "nothing to say" park their probability somewhere stable. Consequence: naive **window-only KV eviction collapses** the moment the first tokens slide out of cache — perplexity explodes even though those tokens seem unimportant.

## Key results

- Fix is 4 tokens: **keep the KV of the initial ~4 "sink" tokens + a rolling recent window** → stable language modeling on **4M+ token** streams (Llama-2, MPT, Falcon, Pythia) with *no fine-tuning*
- **22.2× speedup** over the sliding-window-with-recomputation baseline
- Training-time version: prepend a dedicated learnable sink token

## Impact

Made bounded-memory streaming inference practical; "reserve the sinks" became standard in KV eviction/quantization/compression methods. Follow-up work: sinks act like key biases storing excess attention (they vanish under sigmoid attention), and sinks *prevent over-mixing* of information — connecting to rank-collapse theory ([[Attention is Not All You Need - Rank Collapse (2021)]]).

## Concepts extracted

- [[KV Cache]] — eviction; [[Sliding Window Attention]] — why naive windows fail
