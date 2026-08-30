---
type: concept
topics: [reinforcement-learning, deep-learning]
status: evergreen
created: 2026-08-29
aliases: [reinforcement learning from human feedback, InstructGPT recipe, alignment training]
---

# RLHF

> **Where this sits.** The point where the entire RL cluster meets the [[Transformer]] cluster: [[PPO]] applied to the [[Markov Decision Process|token MDP]], with the reward learned from human preferences. The recipe that turned base language models into assistants.

## The problem: the reward function cannot be written down

RL needs $R$. For "be a helpful, harmless, honest assistant" no one can write it: quality is fuzzy, multi-dimensional, contextual. But humans **recognize** it easily — and they're far more reliable at *comparing* two responses than at scoring one. The founding insight ([[Deep RL from Human Preferences (2017)|Christiano 2017]]): collect **pairwise preferences**, fit a [[Reward Model|reward model]] to them, and optimize the policy against it. In its original RL setting this trained Atari agents and even a simulated-hopper *backflip* — behavior impossible to specify, easy to recognize — from **under 1% of interactions receiving feedback**.

## The pipeline (InstructGPT)

Three stages ([[Training LMs to Follow Instructions - InstructGPT (2022)|Ouyang 2022]]), each producing the input of the next:

1. **SFT** — fine-tune the pretrained model on labeler-written demonstrations: a decent starting policy $\pi_{SFT}$, and the *reference model* everything after is anchored to
2. **Reward model** — sample several responses per prompt, have labelers **rank** them, train an RM on the comparisons ([[Reward Model|Bradley–Terry]]): a network mapping (prompt, response) → scalar
3. **RL** — optimize the policy with [[PPO]] against the RM, over the token MDP (state = prompt + tokens so far, action = next token, reward = RM score at the end, γ = 1 — the [[Markov Decision Process|LLM row]] of the formalism table), maximizing

$$\mathbb{E}\big[\, r_{RM}(x, y) \,\big] \;-\; \beta\, \text{KL}\big(\pi_\theta \,\|\, \pi_{SFT}\big)$$

**The KL penalty is load-bearing**, and it is *not* PPO's clip: the clip keeps $\pi_\theta$ near the *recent* $\pi_{old}$ for estimator validity, resetting every batch; the β-weighted KL keeps it near the *original* $\pi_{SFT}$, forever. Its jobs: stop the policy from wandering into regions where the RM was never trained and is exploitable ([[Reward Model|overoptimization]]), and preserve the pretrained distribution (fluency, knowledge). It admits a clean reading: the KL is a **budget of distribution change**, spent buying reward — empirically, reward gained scales *linearly with √KL from the initial policy* ([[Helpful and Harmless Assistant - Anthropic HH (2022)|Bai 2022]]), so "how far have we moved" literally prices "how much have we gained."

## The results that made it standard

- **Human evaluators preferred the 1.3B InstructGPT's outputs to the 175B GPT-3's** — alignment beat two orders of magnitude of scale; *the* post-training result ([[Training LMs to Follow Instructions - InstructGPT (2022)|Ouyang 2022]])
- Improved truthfulness, reduced toxicity, minimal benchmark regression — and at assistant scale, alignment training *improved* nearly all capability evaluations: the feared "alignment tax" largely absent ([[Helpful and Harmless Assistant - Anthropic HH (2022)|Bai 2022]])
- The same work established **iterated online RLHF** — refresh RM and policy weekly on fresh preferences — attacking the distribution shift that static reward models suffer as the policy moves off their training data

## The costs, honestly

The pipeline holds **four models**: policy, frozen reference, reward model, critic (value head) — memory-heavy and complex, with PPO's [[PPO|implementation sensitivities]] on top. Its failure mode is [[Reward Model|reward hacking]] — the RM is a proxy, and optimizing any proxy hard enough makes true quality *fall* while measured reward rises. Those two pressure points spawned the successor family: [[DPO]] deletes the RM and the RL loop; [[GRPO]] deletes the critic; [[RLVR]] replaces the learned reward with a verifier — each keeping the skeleton while removing one expensive organ.

## Related

- [[PPO]] — the optimizer inside · [[Reward Model]] — the learned R̂ and its pathologies
- [[Policy Gradient]] · [[Actor-Critic]] — the machinery underneath
- [[DPO]] · [[GRPO]] · [[RLVR]] — the descendants
- [[Exploration vs Exploitation]] — entropy/temperature as ACT in the token MDP

## Sources

- [[Deep RL from Human Preferences (2017)]] — the founding recipe
- [[Training LMs to Follow Instructions - InstructGPT (2022)]] — the pipeline and the 1.3B > 175B result
- [[Helpful and Harmless Assistant - Anthropic HH (2022)]] — the √KL law, iterated training, no alignment tax

---
Part of the RL-for-LLMs cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
