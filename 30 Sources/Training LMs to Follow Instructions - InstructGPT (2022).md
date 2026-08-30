---
type: source
source-type: paper
authors: [Long Ouyang, Jeff Wu, Xu Jiang, et al. (OpenAI)]
year: 2022
url: https://consensus.app/papers/details/183f55c332775490b6263da9f4b4204a/?utm_source=claude_desktop
doi: 10.52202/068431-2011
topics: [reinforcement-learning, deep-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [InstructGPT, Ouyang 2022]
---

# Training LMs to Follow Instructions - InstructGPT (2022)

(~23,600 citations — the recipe behind ChatGPT.)

## Summary

The three-step pipeline that defined LLM post-training: **(1) SFT** — fine-tune GPT-3 on labeler-written demonstrations; **(2) reward model** — collect labeler *rankings* of model outputs, train an RM on the comparisons; **(3) RL** — optimize the SFT model against the RM with [[PPO]], plus a **per-token KL penalty against the SFT model** to prevent reward-model exploitation and a pretraining-gradient mix ("PPO-ptx") to limit capability regression.

## Key results

- **Outputs of the 1.3B InstructGPT are preferred by humans to those of the 175B GPT-3** — alignment beat 100× more parameters; the single most-quoted result in post-training
- Improved truthfulness and reduced toxicity with minimal regression on standard NLP benchmarks
- "Making models bigger does not inherently make them better at following a user's intent" — the framing that separated *capability* from *alignment*

## Impact

The canonical instantiation of [[RLHF]]; every chat assistant descends from this pipeline.

## Concepts extracted

- [[RLHF]] — the standard recipe, quantified
