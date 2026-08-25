---
type: source
source-type: paper
authors: [Elena Voita, David Talbot, Fedor Moiseev, Rico Sennrich, Ivan Titov]
year: 2019
url: https://consensus.app/papers/details/771f7b0d9d295c01bdb69d9cedc979d3/?utm_source=claude_desktop
doi: 10.18653/v1/p19-1580
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Voita 2019, specialized heads]
---

# Analyzing Multi-Head Self-Attention (2019)

*Full title: "Analyzing Multi-Head Self-Attention: Specialized Heads Do the Heavy Lifting, the Rest Can Be Pruned"* (ACL 2019, ~1,600 citations).

## Summary

The other half of the redundancy story: the important heads aren't generic — they play **consistent, linguistically interpretable roles**: **positional heads** (attend to fixed relative offsets), **syntactic heads** (track dependency relations like subject→verb), and **rare-token heads**.

## Key results

- Pruning via stochastic gates + differentiable $L_0$ relaxation: **38 of 48 encoder heads removed at only −0.15 BLEU** (WMT En-Ru)
- **Specialized heads are the last to be pruned** — importance and interpretability coincide
- Combined picture with [[Are Sixteen Heads Really Better than One (2019)|Michel 2019]]: a small core of specialists does the work; the rest are trainable scaffolding

## Concepts extracted

- [[Multi-Head Attention]] — specialization
