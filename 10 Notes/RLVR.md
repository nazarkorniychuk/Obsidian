---
type: concept
topics: [reinforcement-learning, deep-learning]
status: evergreen
created: 2026-08-29
aliases: [reinforcement learning with verifiable rewards, reasoning RL, R1-Zero training, test-time compute]
---

# RLVR

> **Where this sits.** The current frontier, and where every thread of the cluster converges: replace the fallible [[Reward Model|reward model]] with a **verifier that cannot be fooled** — exact-match answers, unit tests, rule checkers — and run [[GRPO|critic-free policy optimization]] against it. The result: reasoning that *emerges* from reward alone. Also the cluster's liveliest open controversy.

## The motivation: a reward that can't be hacked

[[Reward Model|Overoptimization]] is the chronic disease of preference-based training: any learned proxy can be mined for errors. But some domains have **ground truth**: a math answer matches or it doesn't; code passes the tests or it doesn't. A verifiable reward is binary, exact, and — within what it checks — *unhackable*. The trade is scope for integrity: verifiers exist only where correctness is checkable (math, code, logic, some science), and they say nothing about *how* the answer was reached — the reward is terminal-only and maximally sparse, the [[Markov Decision Process|hard-credit-assignment corner]] of the token MDP.

## The landmark: R1-Zero — reasoning from reward alone

Take a base model. No reasoning demonstrations, no SFT. Apply [[GRPO]] with rewards for answer correctness and format, and train ([[DeepSeek-R1 (2025)|DeepSeek-AI 2025]]):

- **Reasoning behaviors emerge unprogrammed**: self-reflection, verification of intermediate steps, dynamic strategy switching — and steadily **lengthening chains of thought**, as the policy discovers that spending more tokens thinking buys more reward. The famous "aha moment": mid-solution, the model halts and re-evaluates its own approach
- **Result:** performance on math, competition coding, and STEM **surpassing counterparts trained by supervised learning on human demonstrations** — the ceiling of imitation broken by exploration; the *Nature*-published R1 adds a small cold-start SFT for readability
- **Distillation transfers it**: small models fine-tuned on R1's traces inherit much of the reasoning — emergent patterns, once found, are extractable as data

This is the [[Monte Carlo Tree Search|AlphaGo-lineage]] loop wearing new clothes: **search (long CoT sampling) improves on the raw policy, a verifier stands in for the win condition, and training compresses the search's discoveries back into the model** — with test-time compute as the new search budget.

## The controversy: sharpening or discovery?

The claim "RL teaches models *new* reasoning" met a sharp test — **pass@k at large k**: give base and RL models many attempts and ask who can solve more problems *at all* ([[Does RL Incentivize Reasoning Beyond the Base Model (2025)|Yue 2025]]):

- RLVR models win at pass@1 — but **at large k the base model catches up or overtakes**, across families, algorithms, and benchmarks; the successful reasoning paths *already exist in the base distribution*. On this evidence, standard-scale RLVR mostly **sharpens sampling toward what pretraining already contained** — it converts pass@k into pass@1 — while **distillation genuinely expands the boundary**
- The rebuttals keep it open: reasoning-*path*-aware metrics (CoT-Pass@k) find RLVR does improve the correctness of the chains themselves; **prolonged** RL with KL-resets and diverse tasks reports solving problems the base fails at any k. Honest summary: **elicitation is proven; creation is contested and, at minimum, harder than the headlines implied**
- Sharpest cautionary result in the area: on some models, **spurious — even random — rewards** produce large measured gains, by amplifying high-prior pretrained behaviors through the clipping bias ([[Understanding R1-Zero-Like Training - Dr GRPO (2025)|see also the length-bias audit]]). Gains must be validated across model families before being credited to the method

## The exploration problem, come home

The [[Exploration vs Exploitation]] note's closing pointer lands here. Only *sampled* solutions can be reinforced — so the training loop is a race between exploiting verified successes and keeping the distribution wide enough to keep finding new ones. **Entropy collapse** — the policy narrowing onto a few solution modes, pass@k degrading as pass@1 rises — is the field's central practical failure, attacked from every angle: [[GRPO|Clip-Higher and dynamic sampling]], entropy bonuses, KL-anchor resets, curriculum over problem difficulty. It is, literally, the two-doors coverage problem from the foundations: *the data to reveal a better solution is never generated once the policy stops sampling it.*

## Related

- [[GRPO]] — the algorithm underneath · [[DeepSeek-R1 (2025)]] — the landmark
- [[Reward Model]] — what the verifier replaces, and why
- [[Monte Carlo Tree Search]] — search-then-compress, the older incarnation
- [[Exploration vs Exploitation]] — coverage, now at the frontier

## Sources

- [[DeepSeek-R1 (2025)]] — emergence from pure RL
- [[DeepSeekMath - GRPO (2024)]] — the algorithm and first results
- [[Does RL Incentivize Reasoning Beyond the Base Model (2025)]] — the pass@k critique
- [[Understanding R1-Zero-Like Training - Dr GRPO (2025)]] — base-model controls, length bias
- [[DAPO - Open-Source LLM RL at Scale (2025)]] — open replication and the entropy fixes

---
Part of the RL-for-LLMs cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
