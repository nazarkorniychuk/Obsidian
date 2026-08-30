---
type: source
source-type: paper
authors: [Rafael Rafailov, Archit Sharma, Eric Mitchell, Stefano Ermon, Christopher Manning, Chelsea Finn]
year: 2023
url: https://consensus.app/papers/details/1ee8db4229ee52ec8af09f4e12828ff2/?utm_source=claude_desktop
doi: 10.52202/075280-2338
topics: [reinforcement-learning, deep-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [DPO paper, Rafailov 2023]
---

# Direct Preference Optimization (2023)

(~10,200 citations.)

## Summary

"Your language model is secretly a reward model." The KL-regularized RLHF objective has a **closed-form optimal policy**; inverting it expresses the reward as $r(x,y) = \beta \log\tfrac{\pi(y \mid x)}{\pi_{ref}(y \mid x)} + \text{const}$. Substituting this into the Bradley–Terry preference likelihood makes the reward model *disappear* — the RLHF problem collapses into a **single classification loss on preference pairs**, trained directly on the policy: raise the margin $\beta[\log\tfrac{\pi(y_w)}{\pi_{ref}(y_w)} - \log\tfrac{\pi(y_l)}{\pi_{ref}(y_l)}]$ under a sigmoid.

## Key results

- **Matches or improves on PPO-based RLHF** in the paper's summarization and single-turn dialogue evaluations, and exceeds it at sentiment control — while being *substantially* simpler: no reward model, no rollouts, no critic, no RL loop; stable, lightweight, standard supervised infrastructure
- Later companion work: DPO derived in the token MDP as inverse Q-learning ("from r to Q*")

## Caveats (later literature)

Offline by construction (trains on a fixed preference set → distribution shift as the policy moves); its *implicit* reward generalizes worse out-of-distribution than an explicit RM; and it exhibits its own overoptimization ([[Scaling Laws for Overoptimization in DAAs (2024)]]).

## Concepts extracted

- [[DPO]] — the derivation and the trade
