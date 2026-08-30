---
type: concept
topics: [reinforcement-learning, deep-learning]
status: evergreen
created: 2026-08-29
aliases: [group relative policy optimization, Dr. GRPO, critic-free RL]
---

# GRPO

> **Where this sits.** [[PPO]] with the critic deleted: advantages come from **comparing a group of rollouts against each other** instead of against a learned value function. The algorithm of the reasoning era — [[DeepSeek-R1 (2025)|R1]] runs on it.

## The motivation: the critic is a second LLM

In LLM-scale [[RLHF]], PPO's value function is a full-size model — doubling memory and compute — and it has an unusually thankless job: predict, per token, the expected *terminal* reward of a long generation. Sparse terminal rewards over hundreds of tokens make the critic hard to train well, and a bad baseline poisons the [[Generalized Advantage Estimation|advantages]]. GRPO's bet: for prompts where you can afford several rollouts, **the group is a better baseline than the network**.

## The algorithm

For each prompt, sample a **group of G responses** (G ≈ 8–64) from the current policy, score each (reward model or verifier), and give every token of response $i$ the **group-normalized advantage** ([[DeepSeekMath - GRPO (2024)|Shao 2024]]):

$$\hat{A}_i = \frac{r_i - \text{mean}(r_1, \dots, r_G)}{\text{std}(r_1, \dots, r_G)}$$

— then update with the usual PPO-style clipped ratios + KL-to-reference. Read the advantage: *"how much better was this response than my own other attempts at the same prompt?"* — above group average → all its tokens pushed up; below → down. The foundations cluster names what happened structurally: this is **[[Policy Gradient|REINFORCE with a baseline]]**, where the baseline is the empirical group mean rather than a learned $V(s)$ — i.e. the **λ = 1 Monte Carlo corner** of the [[Generalized Advantage Estimation|GAE dial]], its variance paid down by parallel rollouts instead of a bootstrap. No critic → no bootstrapping → one less [[Temporal Difference Learning#The deadly triad|triad leg]] to manage, at the price of needing G rollouts per prompt (affordable: LLM rollouts are parallel inference).

**Results:** roughly halves training cost vs PPO while *improving* math performance — DeepSeekMath 7B reached **51.7% on competition-level MATH**, approaching GPT-4-era levels at 7B ([[DeepSeekMath - GRPO (2024)|Shao 2024]]); then became the engine of R1's pure-RL reasoning ([[RLVR]]).

## The fine print — where groups fail

- **Degenerate groups give zero signal.** If all G rollouts are correct — or all wrong — every advantage is 0 (and the std blows up numerically near-uniformity): the prompt contributes *nothing*. This is systematic: it silences exactly the too-easy and (worse) the too-hard prompts. **DAPO's** fixes are the practice standard: dynamically resample to keep groups mixed, decouple the clip upward (Clip-Higher) so rare tokens can grow — directly fighting **entropy collapse** — plus token-level loss averaging and overlong-response shaping; with them, an open Qwen-32B system hit **50 on AIME 2024** ([[DAPO - Open-Source LLM RL at Scale (2025)|Yu 2025]])
- **The normalization itself carries a bias.** GRPO's per-response length division and std-normalization interact to **artificially inflate response length — especially of incorrect answers** — so part of "the model learned to think longer" is an optimization artifact. **Dr. GRPO** removes the bias, matching reasoning performance with far better token efficiency ([[Understanding R1-Zero-Like Training - Dr GRPO (2025)|Liu 2025]]). A clean instance of the vault's recurring lesson: objective-function details silently masquerade as "emergent behavior"
- Credit assignment is coarse by construction: every token in a response inherits the same advantage — the sequence-level signal of a bandit, not the per-step signal a critic could in principle give ([[Markov Decision Process|the hard-credit-assignment, easy-dynamics MDP]])

## Related

- [[PPO]] — everything kept except the critic
- [[Policy Gradient]] · [[Generalized Advantage Estimation]] — what group-normalization *is*
- [[RLVR]] — the reward that made GRPO shine
- [[Exploration vs Exploitation]] — entropy collapse, the ACT-side failure

## Sources

- [[DeepSeekMath - GRPO (2024)]] — the algorithm
- [[DAPO - Open-Source LLM RL at Scale (2025)]] — the practical fixes
- [[Understanding R1-Zero-Like Training - Dr GRPO (2025)]] — the length bias
- [[DeepSeek-R1 (2025)]] — at scale

---
Part of the RL-for-LLMs cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
