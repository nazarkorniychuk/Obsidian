---
type: source
source-type: paper
authors: [DeepSeek-AI (Daya Guo, Dejian Yang, et al.)]
year: 2025
url: https://consensus.app/papers/details/3b6645a38cd55272bd633b72b0c8acfa/?utm_source=claude_desktop
doi: 10.1038/s41586-025-09422-z
topics: [reinforcement-learning, deep-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [R1, R1-Zero, DeepSeek-R1]
---

# DeepSeek-R1 (2025)

(~5,600 citations for the *Nature* version + ~5,100 for the arXiv report.)

## Summary

The demonstration that **reasoning can be incentivized by pure RL** — no human-labelled reasoning trajectories. **R1-Zero**: take the base model, apply [[GRPO]] with **verifiable rewards only** (answer correctness + format), and watch advanced reasoning patterns *emerge*: self-reflection, verification, dynamic strategy adaptation, and steadily lengthening chains of thought — including the famous mid-solution "aha moment" re-evaluations. **R1** adds a small cold-start SFT and multi-stage training for readability and generality.

## Key results

- Superior performance on verifiable tasks (mathematics, competition coding, STEM), **surpassing counterparts trained by supervised learning on human demonstrations** — the reasoning ceiling of imitation broken by exploration
- The emergent long-CoT behavior was *not programmed*: response length and reflection frequency grow during training as the policy discovers that thinking longer pays reward
- **Distillation transfers it**: small models fine-tuned on R1's traces inherit much of the reasoning — emergent patterns are extractable as data

## Impact

Defined the [[RLVR]] era and made [[GRPO]] the field's default; the open replication wave (and its critical re-examinations) is the current frontier.

## Concepts extracted

- [[RLVR]] — the landmark
- [[GRPO]] — at scale
