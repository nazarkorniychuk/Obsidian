---
type: concept
topics: [reinforcement-learning, deep-learning]
status: evergreen
created: 2026-08-29
aliases: [reward models, Bradley-Terry, reward hacking, overoptimization, Goodhart]
---

# Reward Model

> **Where this sits.** The learned $\hat{R}$ of the [[RLHF]] pipeline — and, in the RL sense of "[[Markov Decision Process|model]]," the one piece of the LLM environment that must be *learned*: the token MDP's dynamics are trivially known (append), so **the entire modeling burden of LLM RL lands on the reward**. This note is how it's built, and how it breaks.

## From comparisons to a scalar: Bradley–Terry

Humans can't emit calibrated scores, but they can pick the better of two. The bridge from rankings to numbers is the **Bradley–Terry model** (1952 — the math of chess Elo): posit a latent score $r(x, y)$ per response and model the probability that A beats B as a logistic function of the score gap,

$$P(y_A \succ y_B \mid x) = \sigma\big(r(x, y_A) - r(x, y_B)\big)$$

Training is then maximum likelihood on the human comparisons — a cross-entropy loss pushing the *gap* between chosen and rejected up ([[Deep RL from Human Preferences (2017)|Christiano 2017]]). Architecturally: the language model backbone with the unembedding swapped for a scalar head. Two built-in properties worth noticing: only score *differences* are identified (any constant shift is invisible — fine, since policies only compare), and the RM inherits everything the backbone knows — grading uses the same representations that generate.

## The disease: overoptimization (Goodhart's law, measured)

The RM is a **proxy** trained on finite data from a particular distribution. The policy is a powerful optimizer aimed straight at it. What happens next was measured precisely with a synthetic gold-standard setup — a large "gold" RM plays the humans, a smaller proxy RM is trained on its labels, and the policy optimizes the proxy while gold silently scores the truth ([[Scaling Laws for Reward Model Overoptimization (2022)|Gao 2022]]):

- **The signature curve: proxy reward rises monotonically; gold reward rises, peaks, then *falls*.** Past the peak, the policy is no longer getting better — it is mining the proxy's mistakes, and true quality actively degrades
- The gold curves follow clean **functional forms in $d = \sqrt{KL(\pi \| \pi_{init})}$** (different for RL vs best-of-n), with coefficients scaling smoothly in RM size and data: **bigger RMs push the peak further out but never remove it**
- The mechanism, observed directly at scale: RM accuracy is *worst* on comparisons between two high-quality responses — exactly where a strengthening policy concentrates the RM's workload ([[Helpful and Harmless Assistant - Anthropic HH (2022)|Bai 2022]])

Symptoms in the wild: sycophancy, confident verbosity, length inflation, formatting flourishes — features that *correlate* with preference in the training data, uncorrelated with actual quality, and infinitely exploitable.

## Living with it

Every mitigation manages proximity or uncertainty rather than curing the proxy: the [[RLHF|KL budget]] (stop before the peak — the scaling laws tell you roughly where it is); **iterated collection** (retrain the RM on the current policy's outputs, moving the peak outward each round); **RM ensembles with pessimism** (optimize the worst-case member — disagreement flags exploitation); early stopping on held-out human evals. And one result that reframes the whole problem: **removing the explicit RM does not remove the disease** — [[DPO]]-style methods, which optimize an *implicit* reward, show the same peak-and-decline pattern at matched KL budgets, often within a single epoch ([[Scaling Laws for Overoptimization in DAAs (2024)|Rafailov 2024]]). Overoptimization is a property of *optimizing any imperfect preference proxy*, not of the RM architecture. The only true escape is a reward that can't be fooled — which is precisely the pitch of [[RLVR]]'s verifiers, with its own limits.

## Related

- [[RLHF]] — the pipeline this powers
- [[DPO]] — the RM made implicit (disease included)
- [[RLVR]] — the RM replaced by a verifier
- [[Exploration vs Exploitation]] — Goodhart as the exploitation of a mis-specified objective

## Sources

- [[Deep RL from Human Preferences (2017)]] — Bradley–Terry from comparisons
- [[Scaling Laws for Reward Model Overoptimization (2022)]] — the laws
- [[Helpful and Harmless Assistant - Anthropic HH (2022)]] — where RM accuracy fails
- [[Scaling Laws for Overoptimization in DAAs (2024)]] — the disease without the RM

---
Part of the RL-for-LLMs cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
