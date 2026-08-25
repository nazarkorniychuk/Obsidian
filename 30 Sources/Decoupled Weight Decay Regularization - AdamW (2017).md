---
type: source
source-type: paper
authors: [Ilya Loshchilov, Frank Hutter]
year: 2017
url: https://consensus.app/papers/details/851939bbb9015bd3a52e914e02157451/?utm_source=claude_desktop
doi: 10.48550/arxiv.1711.05101
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [AdamW paper, Loshchilov & Hutter 2017]
---

# Decoupled Weight Decay Regularization - AdamW (2017)

(~38,000 citations.)

## Summary

Exposes a silent bug in standard practice: for SGD, adding an $L_2$ penalty $\tfrac{\lambda}{2}\|\theta\|^2$ to the loss is equivalent to weight decay $\theta \leftarrow (1-\eta\lambda)\theta$ — but **for Adam they are *not* equivalent**: the $L_2$ gradient $\lambda\theta$ flows through Adam's $\sqrt{\hat v}$ normalizer, so parameters with large gradient history get *less* regularization — precisely backwards. **AdamW** decouples: apply decay directly to the weights, outside the adaptive machinery:

$$\theta_{t+1} = \theta_t - \eta\Big(\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon} + \lambda\,\theta_t\Big)$$

## Key results

- Decouples the optimal $\lambda$ from the learning-rate choice (independent tuning)
- **Substantially improves Adam's generalization**, closing the gap to momentum-SGD on image classification where Adam had been losing
- Same authors' SGDR introduced **cosine-annealing (warm-restart) schedules** — the other half of the modern recipe

## Impact

The literal optimizer string in virtually every LLM training config ("AdamW, β=(0.9, 0.95), cosine schedule"). Later theory: AdamW implicitly performs $\ell_\infty$-constrained optimization (weights bounded by $1/\lambda$), and its dynamics asymptotically approximate SGD — candidate explanations for the generalization gain.

## Concepts extracted

- [[Adam Optimizer]] — the correction that made Adam the LLM default
