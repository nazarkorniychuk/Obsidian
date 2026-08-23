---
type: source
source-type: paper
authors: [FengTing Liao, et al.]
year: 2026
url: https://consensus.app/papers/details/59c387576d5a5f0b8fcf67345243b019/?utm_source=claude_desktop
doi: 10.48550/arxiv.2602.06471
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Liao 2026, hourglass FFN paper]
---

# Revisiting the Shape Convention of Transformer Language Models (2026)

## Summary

Challenges the narrow-wide-narrow FFN convention. Documents that dense transformer LMs allocate most parameters to the MLP at **expansion ratios between 2 and 4**, then tests inverting the shape: deeper stacks of **wide-narrow-wide (hourglass) sub-MLPs** with residual connections.

## Key results

- **Hourglass FFNs outperform conventional FFNs up to 400M parameters**; comparable at 1B
- Reallocating parameters saved by lighter hourglass FFNs into larger hidden dimensions gives **consistent improvements at matched budgets**
- Trading FFN parameters for attention parameters also improves over the conventional split — the standard attention:FFN = 1:2 allocation is not optimal everywhere

## Why it's in the vault

The documented reference for the **2–4× expansion-ratio convention**, and current evidence that both the FFN's shape and the attention/FFN split are under-optimized conventions rather than settled results.

## Concepts extracted

- [[Feedforward Network]] — expansion ratio section
