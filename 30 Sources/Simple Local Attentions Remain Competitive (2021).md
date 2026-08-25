---
type: source
source-type: paper
authors: [Wenhan Xiong, et al. (Meta)]
year: 2021
url: https://consensus.app/papers/details/2444918f6c815c1785f468e48ead5f1d/?utm_source=claude_desktop
doi: 10.18653/v1/2022.naacl-main.144
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Xiong 2021, local attention replication]
---

# Simple Local Attentions Remain Competitive (2021)

*Full title: "Simple Local Attentions Remain Competitive for Long-Context Tasks."*

## Summary

The controlled replication the efficient-attention field needed: pre-train large models with each attention variant **on the same corpus with the same recipe**, then fine-tune on real long-context tasks — isolating the attention mechanism from confounds.

## Key results

- **None of the tested efficient attentions beat a simple local window attention** under the standard pretrain-finetune paradigm
- Exposed pitfalls in the widely-used Long Range Arena benchmark — LRA rankings don't predict real-task performance
- Even the customary window *overlap* is unnecessary: disjoint local attention matched Longformer QA performance at **half the pretraining compute**

## Why it's in the vault

The clean negative result behind the efficient-attention failure story: the sophisticated variants (kernels, LSH, low-rank) added complexity without beating the dumbest baseline. Together with [[FlashAttention (2022)]] (which made exact attention fast) it explains why the X-former zoo went extinct at the frontier.

## Concepts extracted

- [[Attention Mechanism]] — failures section
