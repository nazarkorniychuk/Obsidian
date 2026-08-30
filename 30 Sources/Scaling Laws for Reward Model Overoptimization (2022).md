---
type: source
source-type: paper
authors: [Leo Gao, John Schulman, Jacob Hilton]
year: 2022
url: https://consensus.app/papers/details/ad786892bdfa54bc8adcaaccb4527212/?utm_source=claude_desktop
doi: 10.48550/arxiv.2210.10760
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [Gao 2022, overoptimization scaling laws]
---

# Scaling Laws for Reward Model Overoptimization (2022)

(~1,200 citations.)

## Summary

Measures Goodhart's law quantitatively. Setup: a large fixed **"gold" RM plays the role of humans**; a smaller **proxy RM** is trained on its labels; the policy is then optimized against the proxy while the gold model silently scores the truth.

## Key results

- The signature curve: as optimization proceeds, **proxy reward rises monotonically while gold reward rises, peaks, and then *falls*** — past the peak, the policy is learning to exploit the proxy's errors, and true quality actively degrades
- The gold-reward curves follow clean **functional forms in $d = \sqrt{KL(\pi \| \pi_{init})}$** — different for RL ($d(\alpha - \beta d)$) vs best-of-n sampling — with coefficients scaling smoothly in proxy-RM parameters: **bigger RMs and more RM data push the peak further out, but never remove it**
- KL-penalty strength, policy size, and data size all mapped

## Impact

Turned "reward hacking" from anecdote into a measurable, predictable phenomenon with scaling laws — the quantitative core of [[Reward Model]] practice (how far you may optimize, when to stop, why to keep collecting preferences).

## Concepts extracted

- [[Reward Model]] — the overoptimization laws
- [[RLHF]] — why the KL budget exists
