---
type: source
source-type: paper
authors: [Adrien Ali Taiga, William Fedus, Marlos C. Machado, Aaron Courville, Marc G. Bellemare]
year: 2020
url: https://consensus.app/papers/details/2e26e8393ad652ceb989d403406b4d4e/?utm_source=claude_desktop
doi: 10.48550/arxiv.2109.11052
topics: [reinforcement-learning]
status: processed
rating: 3
created: 2026-08-27
aliases: [Taiga 2020]
---

# On Bonus Based Exploration Methods - Taiga (2020)

(~74 citations. The honest-benchmark paper for exploration bonuses.)

## Summary

Re-evaluates the popular exploration bonuses (pseudo-counts, ICM, RND, NoisyNets) in a **common harness** (Rainbow) across Montezuma's Revenge, the hard-exploration set, and the full Atari-57 suite — instead of each method's bespoke setup.

## Key results

- Bonuses raise Montezuma's Revenge scores but provide **no meaningful gain over ε-greedy** across the wider suite; the best Montezuma methods often *underperform* ε-greedy on easy-exploration games
- Conclusion holds after per-regime hyperparameter tuning; **none of the methods benefit from 1B frames** (vs 200M) on the hard-exploration set
- Attribution: much of the reported Montezuma progress came from **architecture changes, not better exploration**

## Why it's in the vault

Same role as [[Fantastic Pretraining Optimizers II - Hyperball (2026)|Hyperball]] for optimizers: headline wins on a single showcase benchmark routinely evaporate under a fair harness. Single-domain progress ≠ method progress.

## Concepts extracted

- [[Exploration vs Exploitation]] — the benchmark-honesty caveat
