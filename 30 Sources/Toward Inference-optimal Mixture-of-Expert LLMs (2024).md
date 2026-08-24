---
type: source
source-type: paper
authors: [Longfei Yun, et al.]
year: 2024
url: https://consensus.app/papers/details/376422496f9b5b00905525f04a502304/?utm_source=claude_desktop
doi: 10.48550/arxiv.2404.02852
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Yun 2024]
---

# Toward Inference-optimal Mixture-of-Expert LLMs (2024)

## Summary

Amends MoE scaling laws by adding **inference efficiency** as a metric alongside validation loss — because loss-optimal configurations (scale experts until saturation, training FLOPs stay constant) produce models that are expensive to *serve*.

## Key results

- Diminishing returns in expert count confirmed — but pure training-loss optimization says "add experts anyway," which inference cost forbids
- **4–8 experts: most serving-efficient at matched performance, but 2.5–3.5× more expensive to train**
- **16/32 experts on a 70–85% smaller model with more training data**: the better setup under a fixed training budget
- Bottom line: train-time-optimal ≠ serve-time-optimal; the right config depends on which bill you pay

## Concepts extracted

- [[Mixture of Experts]] — expert-count tradeoff
