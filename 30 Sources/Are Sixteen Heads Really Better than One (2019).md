---
type: source
source-type: paper
authors: [Paul Michel, Omer Levy, Graham Neubig]
year: 2019
url: https://consensus.app/papers/details/fcd8597261675e4ab2fd9ecb5d04e8f7/?utm_source=claude_desktop
doi: 10.48550/arxiv.1905.10650
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Michel 2019, sixteen heads]
---

# Are Sixteen Heads Really Better than One (2019)

(NeurIPS 2019, ~1,450 citations.)

## Summary

The head-redundancy landmark: after training, **a large percentage of attention heads can be removed at test time with no significant performance loss** — some layers reduce to a *single* head. Greedy importance-based pruning yields speed/memory gains.

## Key results

- Head importance is extremely uneven; encoder-decoder *cross*-attention heads are the most sensitive, self-attention heads the most prunable
- Preliminary evidence that multi-head's value lies in **training dynamics** — many heads help optimization even if the trained function doesn't need them

## Why it's in the vault

Reframes the "why $h$ heads?" question: not because inference needs $h$ parallel patterns, but (at least partly) because training does. Companion result: [[Analyzing Multi-Head Self-Attention (2019)|Voita 2019]].

## Concepts extracted

- [[Multi-Head Attention]] — redundancy
