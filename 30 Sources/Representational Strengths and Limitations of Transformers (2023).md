---
type: source
source-type: paper
authors: [Clayton Sanford, Daniel Hsu, Matus Telgarsky]
year: 2023
url: https://consensus.app/papers/details/64cc009e35c557f7a622bb24235d8a7f/?utm_source=claude_desktop
doi: 10.48550/arxiv.2306.02896
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Sanford 2023]
---

# Representational Strengths and Limitations of Transformers (2023)

(NeurIPS 2023, ~160 citations.)

## Summary

Communication-complexity analysis of what attention is *for* — separations against RNNs/FFNs in both directions.

## Key results

- **Strength — sparse averaging** ("average the values at the $q$ positions your query points to"): transformers need size scaling only **logarithmically** in input length; RNNs and FFNs need **polynomial** scaling. This is the cleanest formal statement of *why* attention beats recurrence at content-based long-range retrieval — and it also proves the **necessity of a large embedding dimension** ($d$ must grow with the task's sparsity parameter)
- **Limitation — triple detection** ("do any three positions $i,j,k$ satisfy a joint relation?"): attention layers need size scaling **linearly** in input length — pairwise $QK^\top$ machinery can't cheaply see third-order interactions
- The pairwise-only limitation is intrinsic: attention computes 2-way inner products; 3-way relations require either depth tricks or luck

## Concepts extracted

- [[Attention Mechanism]] — what attention is provably good and bad at
