---
type: source
source-type: paper
authors: [Yanqi Zhou, et al. (Google)]
year: 2022
url: https://consensus.app/papers/details/ae8eb98b3c005a1da7c1b6bafd8bd3b8/?utm_source=claude_desktop
doi: 10.52202/068431-0515
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Expert Choice routing, Zhou 2022]
---

# Mixture-of-Experts with Expert Choice Routing (2022)

(~860 citations.)

## Summary

Inverts the routing question: instead of **tokens choosing their top-k experts** (which makes balance a fight against the loss), **each expert chooses its top-k tokens** from the batch. Load balance holds **by construction** — every expert fills exactly its bucket — so no auxiliary balancing loss is needed at all. Side effect: tokens receive a *variable* number of experts (important tokens can get many, easy tokens few or none).

## Key results

- **>2× faster training convergence** than Switch top-1 and GShard top-2 at equal compute
- Better GLUE/SuperGLUE fine-tuning at the same cost; beats dense T5 on 7/11 tasks at *smaller* activation cost

## The catch

Expert-choice requires experts to compare tokens **across the whole batch/sequence** — during autoregressive decoding, future tokens aren't available and batch composition leaks information across examples. This causality problem is why decoder LLMs stayed with token-choice + balancing machinery, while expert choice thrives in encoders and vision (where it generally beats token choice).

## Concepts extracted

- [[Mixture of Experts]] — the balance-by-construction alternative
