---
type: source
source-type: paper
authors: [Shibani Santurkar, Dimitris Tsipras, Andrew Ilyas, Aleksander Madry]
year: 2018
url: https://consensus.app/papers/details/eff376de66ee5c64a82e3312413a08c1/?utm_source=claude_desktop
doi: 10.48550/arxiv.1805.11604
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Santurkar 2018]
---

# How Does Batch Normalization Help Optimization (2018)

*Subtitle: "(No, It Is Not About Internal Covariate Shift)"* (NeurIPS 2018, ~1,800 citations).

## Summary

Direct experimental test of BatchNorm's stated mechanism — and its refutation. Injecting *non-stationary noise* after BN layers (deliberately re-creating severe distribution shift) leaves BN's benefits intact; measured "covariate shift" barely correlates with performance.

## Key results

- BN's actual effect: it makes the **loss landscape significantly smoother** — improved Lipschitzness of loss and gradients → gradients are more predictive → larger learning rates are safe → faster training
- The famous named mechanism ("internal covariate shift") was marketing that outlived its own falsification

## Why it's in the vault

The canonical example of a **right technique with a wrong explanation** — the same epistemic pattern as Shazeer's "divine benevolence" (SwiGLU) later resolved by NTK conditioning, and a caution for every mechanism claim in this vault: landscape-smoothing/conditioning keeps turning out to be the real story behind "it just trains better."

## Concepts extracted

- [[Normalization]] — the real mechanism
