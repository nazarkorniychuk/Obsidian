---
type: concept
topics: [reinforcement-learning, deep-learning]
status: evergreen
created: 2026-08-29
aliases: [direct preference optimization, direct alignment, DAA]
---

# DPO

> **Where this sits.** The most radical simplification of [[RLHF]]: a derivation showing the reward model and the RL loop can *both* be deleted — the whole pipeline collapses into **one supervised loss on preference pairs**. "Your language model is secretly a reward model."

## The motivation: RLHF's machinery is heavy

Four models in memory (policy, reference, RM, critic), PPO's [[PPO|implementation sensitivities]], online rollouts every step. The question DPO asks: is all of that machinery *necessary*, or is it one particular way of solving an optimization problem that has a shortcut?

## The derivation, in three moves

**Move 1 — the RLHF objective has a closed-form solution.** For the KL-regularized objective $\max_\pi \mathbb{E}[r(x,y)] - \beta\,\text{KL}(\pi \| \pi_{ref})$, the optimal policy is known exactly (a classical result): reweight the reference by exponentiated reward,

$$\pi^*(y \mid x) \propto \pi_{ref}(y \mid x)\, e^{\, r(x,y)/\beta}$$

**Move 2 — invert it.** Solve for the reward instead: $r(x,y) = \beta \log \tfrac{\pi^*(y|x)}{\pi_{ref}(y|x)} + \text{const}$. Read that carefully — **every policy implicitly defines a reward function** (how much it up-weights each response relative to the reference), and the optimal policy's implicit reward *is* the true one.

**Move 3 — substitute into Bradley–Terry.** The [[Reward Model|preference likelihood]] depends only on reward *differences*, so the constant cancels, the reward model vanishes as a separate object, and maximum likelihood on the human comparisons becomes a loss directly on the policy ([[Direct Preference Optimization (2023)|Rafailov 2023]]):

$$L_{DPO} = -\mathbb{E}_{(x, y_w, y_l)}\Big[\log \sigma\Big(\beta \log \tfrac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \beta \log \tfrac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)}\Big)\Big]$$

In words: **increase the chosen response's probability relative to the reference, decrease the rejected one's, with logistic pressure on the margin.** No sampling during training, no rollouts, no critic, no separate RM — a classification loss on a fixed dataset, running on ordinary SFT infrastructure. The β keeps its RLHF meaning (implicit KL strength).

## Results and adoption

- **Matches or beats PPO-based RLHF** in the paper's summarization and dialogue evaluations, exceeds it at sentiment control — with dramatically less complexity and no RL instability ([[Direct Preference Optimization (2023)|Rafailov 2023]])
- Became the default alignment method of the open-model world almost overnight: preference tuning at SFT cost

## The honest ledger

- **It's offline.** DPO trains on a *fixed* preference set; as the policy moves, its outputs drift from where the data was collected — the very distribution shift online RLHF handles by sampling fresh rollouts each step. (Iterated/online DPO variants re-close this gap by re-collecting.)
- **The disease survived the surgery.** DPO's implicit reward is still a proxy fit to finite preferences — and it **overoptimizes just like RLHF**: at matched KL budgets, the same true-quality peak-and-decline, often within one epoch ([[Scaling Laws for Overoptimization in DAAs (2024)|Rafailov 2024]] — from the DPO authors). Deleting the reward model didn't repeal [[Reward Model|Goodhart]]
- **The implicit RM generalizes worse** out-of-distribution than an explicitly trained one — part of why frontier labs largely kept explicit RMs + online RL while the open ecosystem ran on DPO
- Structural reading via the [[Reinforcement Learning|loop]]: DPO keeps EVALUATE-from-data and IMPROVE but *deletes ACT* — no fresh experience is ever generated. It is the offline corner of LLM post-training, with offline RL's characteristic strengths (cheap, stable) and weakness (bounded by the data you already have)

## Related

- [[RLHF]] — the pipeline this compresses
- [[Reward Model]] — made implicit, pathologies included
- [[Value-Based vs Policy-Based RL]] — the offline-RL parallel
- [[GRPO]] — the other simplification path (keep RL, delete the critic)

## Sources

- [[Direct Preference Optimization (2023)]] — the derivation and results
- [[Scaling Laws for Overoptimization in DAAs (2024)]] — the limits

---
Part of the RL-for-LLMs cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
