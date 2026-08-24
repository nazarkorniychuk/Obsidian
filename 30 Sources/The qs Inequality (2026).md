---
type: source
source-type: paper
authors: [Vignesh Adhinarayanan, et al.]
year: 2026
url: https://consensus.app/papers/details/279c4b3cb55c5c92bc911fdac4911e52/?utm_source=claude_desktop
doi: 10.48550/arxiv.2603.08960
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Adhinarayanan 2026, MoE double penalty]
---

# The qs Inequality (2026)

*Full title: "The qs Inequality: Quantifying the Double Penalty of Mixture-of-Experts at Inference."*

## Summary

The contrarian systems result: MoE's training-FLOP efficiency can **invert at inference**. Two structural penalties during decoding:

1. **Reuse fragmentation** — routing splits microbatches across experts, so each expert's weights are streamed from memory for only a few tokens (poor weight reuse → bandwidth-bound)
2. **KV-cache crowding** — the huge resident expert pool eats HBM that dense models would give to the [[KV Cache]], hurting long-context serving

## Key results

- The **qs criterion**: sparsity $s$ (active fraction) vs quality-equivalence factor $q$ (how much bigger a dense model must be to match) predicts when MoE loses to a quality-matched dense model at serving time
- **DeepSeek-V3 at 128k context: a quality-matched dense baseline has 4.5× throughput advantage**; Switch-C becomes infeasible on clusters where matched dense still runs
- Suggested reading: MoE as a *training-time* optimization; distill to dense for inference-efficient deployment

## Caveat

Analysis/modeling paper; serving stacks (expert offload, wide batching) partially mitigate in practice.

## Concepts extracted

- [[Mixture of Experts]] — the inference cost counterweight
