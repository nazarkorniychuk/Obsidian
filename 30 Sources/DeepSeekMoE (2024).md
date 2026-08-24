---
type: source
source-type: paper
authors: [Damai Dai, et al. (DeepSeek-AI)]
year: 2024
url: https://consensus.app/papers/details/5eb7528521345f40a2eefa071655e8dc/?utm_source=claude_desktop
doi: 10.48550/arxiv.2401.06066
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [Dai 2024, fine-grained shared experts paper]
---

# DeepSeekMoE (2024)

*Full title: "DeepSeekMoE: Towards Ultimate Expert Specialization in Mixture-of-Experts Language Models"* (~1,000 citations).

## Summary

The architecture behind DeepSeek-V2/V3. Diagnosis: conventional top-K-of-N MoE (GShard-style) fails at **expert specialization** — experts learn overlapping knowledge. Two fixes:

1. **Fine-grained segmentation**: split each expert into $m$ smaller ones ($mN$ experts, activate $mK$) → combinatorially more expert mixtures at the same compute
2. **Shared experts**: isolate $K_s$ always-active experts to absorb *common* knowledge, so routed experts don't all redundantly learn it

## Key results

- DeepSeekMoE-2B ≈ GShard-2.9B (which has **1.5× its expert params and compute**); nearly matches the *dense* upper bound of equal total params
- **16B matches LLaMA-2-7B at ~40% of the compute**
- 145B preliminary: matches dense DeepSeek-67B at 28.5% (possibly 18.2%) of compute

## Impact

Made fine-grained + shared the modern MoE template (DeepSeek-V2/V3, Qwen-MoE, Ling); empirical companion to the granularity scaling law ([[Scaling Laws for Fine-Grained Mixture of Experts (2024)]]).

## Concepts extracted

- [[Mixture of Experts]] — fine-grained + shared-expert variant
