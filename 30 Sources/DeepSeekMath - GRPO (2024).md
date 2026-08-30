---
type: source
source-type: paper
authors: [Zhihong Shao, Peiyi Wang, Qihao Zhu, et al. (DeepSeek)]
year: 2024
url: https://consensus.app/papers/details/23b0c4c4acd4568996860352de60446e/?utm_source=claude_desktop
doi: 10.48550/arxiv.2402.03300
topics: [reinforcement-learning, deep-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [GRPO paper, DeepSeekMath, Shao 2024]
---

# DeepSeekMath - GRPO (2024)

(~8,300 citations.)

## Summary

Introduces **Group Relative Policy Optimization**: PPO with the critic deleted. Sample a *group* of G responses per prompt, score each, and set every response's advantage to its **group-normalized reward** $(r_i - \bar{r})/\sigma_r$ — the group's own mean replaces the learned value baseline. Keeps PPO's per-token ratios, clipping, and KL regularization.

## Key results

- Motivation was **memory**: PPO's critic is a second full-size LLM; GRPO removes it, cutting training cost roughly in half while *improving* results on math
- DeepSeekMath 7B (math pretraining + GRPO): **51.7% on competition-level MATH** — approaching GPT-4/Gemini-Ultra levels at 7B scale; 60.9% with self-consistency

## Impact

The algorithm of the reasoning era: [[DeepSeek-R1 (2025)|R1]] and most open reasoning models train with GRPO or a variant. Structurally it is the [[Policy Gradient|REINFORCE-with-baseline]] idea reborn — Monte Carlo advantages (λ = 1), variance paid down by parallel rollouts instead of a critic.

## Concepts extracted

- [[GRPO]] — the algorithm
