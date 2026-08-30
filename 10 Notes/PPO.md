---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [Proximal Policy Optimization, PPO-clip, clipped surrogate objective, TRPO]
---

# PPO

> **Where this sits.** The workhorse of the [[Policy Gradient|policy-gradient]] branch and the algorithm of [[RLHF]]. Its entire content is a safe answer to one question: **how do you take more than one gradient step on the same batch of on-policy data without destroying the policy?**

## The problem: policy gradients waste their data — and big steps kill

Two compounding pains of the vanilla [[Actor-Critic|actor-critic]] setup:

**1. One update per sample.** The policy gradient is an expectation under the *current* policy. After a single gradient step, θ has moved — the batch you just collected is now another policy's data, and the estimator is no longer valid. Strictly, you collect → step once → throw everything away. Given that collecting is the expensive part (weeks of simulated time — [[High-Dimensional Continuous Control Using GAE (2015)|the GAE ledger]]), single-use data is brutal.

**2. A too-big step is not just slow to fix — it's self-poisoning.** In supervised learning a bad step is fine: the dataset is fixed, the next steps walk back. In on-policy RL **the policy generates the next dataset.** Step too far → policy degrades → *it collects degraded data* → gradients computed on garbage → further degradation. A death spiral, from one oversized step. Worse, the danger is invisible in parameter space: the gradient is a local approximation, and a small change in θ can be a *huge* change in the action distribution (probabilities are exponentials of logits). Step size in θ is the wrong dial; what needs limiting is the step in *policy space*.

So the goal is precise: **squeeze several epochs of updates out of each batch (fix 1), while guaranteeing the policy never moves far from the one that collected the data (fix 2).**

## First answer: TRPO — the trust region, done rigorously

Two ingredients ([[Trust Region Policy Optimization (2015)|Schulman 2015]]):

- **Importance sampling makes stale data legal.** You can estimate the *new* policy's performance on the *old* policy's data by reweighting each sample with the probability **ratio** $r_t(\theta) = \tfrac{\pi_\theta(a_t \mid s_t)}{\pi_{\text{old}}(a_t \mid s_t)}$ — "how much more (or less) likely would *I* have been to do what the old policy did." The surrogate objective $\mathbb{E}[\, r_t(\theta)\, \hat{A}_t\,]$ is valid — *but only near $\pi_{\text{old}}$*; the reweighting degrades as the policies diverge
- **So constrain the divergence:** maximize the surrogate subject to $\overline{\text{KL}}(\pi_{\text{old}} \| \pi_\theta) \le \delta$ — a **trust region** in policy space, not parameter space. The theory: this bounds the surrogate's error, yielding **guaranteed monotonic improvement**

**Results:** robust learning of swimming/hopping/walking gaits and Atari **with little hyperparameter tuning** — the reliability that made trust regions standard. **The catch:** enforcing the KL constraint needs second-order machinery (conjugate gradient + line search) — complex, expensive, and incompatible with parameter sharing (policy+value in one network) and everyday architecture choices. Right idea, wrong price.

## PPO: the trust region as a loss function

Keep TRPO's ratio-times-advantage surrogate; replace the constraint with a **clip inside the objective** ([[Proximal Policy Optimization (2017)|Schulman 2017]]):

$$L^{CLIP}(\theta) = \mathbb{E}_t\Big[\min\big(\, r_t(\theta)\,\hat{A}_t,\;\; \text{clip}(r_t(\theta),\, 1{-}\epsilon,\, 1{+}\epsilon)\,\hat{A}_t \,\big)\Big], \qquad \epsilon = 0.2$$

Walk the logic, separately for the two signs of the advantage:

![[ppo-clip-objective.png|560]]

- **Good action ($\hat{A} > 0$):** the objective rewards raising the ratio — but only up to $1{+}\epsilon$, where the clip makes it **flat**. Beyond that point the gradient is *zero*: no incentive to push a good action's probability further on this batch. Enthusiasm, capped
- **Bad action ($\hat{A} < 0$):** by symmetry you'd expect the penalty to flatten below $1{-}\epsilon$ — and for *reducing* the ratio it does. But look at the right side of the plot: if the ratio has (through other updates) *risen* on a bad action, the **min picks the unclipped branch, and the penalty keeps growing**. This is the crucial asymmetry: the min makes the bound **one-sided pessimistic** — clipping only ever *removes reward* for moving too far; it never shields a move that makes things worse. Mistakes always generate corrective gradient; wins stop paying at the boundary

The payoff: with each sample's influence bounded this way, it becomes safe to run **K epochs (3–10) of minibatch SGD on the same batch** — the sample reuse that vanilla PG forbids, obtained *without* a replay buffer and without leaving the on-policy regime (a batch is reused a few epochs, then discarded — contrast [[Deep Q-Network|DQN]]'s million-step buffer).

**The full loss** in practice adds two terms: $L = L^{CLIP} - c_1\, L^{VF} + c_2\, H(\pi_\theta)$ — the critic's value loss (shared network, one optimizer — the thing TRPO couldn't do) and the **entropy bonus** paying the policy to stay stochastic ([[Exploration vs Exploitation|exploration, built into the loss]]).

**Results:** outperforms the other online policy-gradient methods on MuJoCo continuous control and Atari; overall the best balance of **sample complexity, simplicity, and wall-time** ([[Proximal Policy Optimization (2017)|Schulman 2017]]). First-order only — it runs wherever Adam runs.

## The implementation details that matter (more than you'd like)

Two honest-attribution studies took PPO apart:

- **The code-level optimizations are load-bearing.** PPO's reference implementation ships extras absent from the paper — value-function clipping, reward scaling, orthogonal initialization, learning-rate annealing, observation normalization. Ablations show these account for **most of PPO's cumulative-reward advantage over TRPO**, and that they — not only the clip — do much of the work of keeping successive policies close ([[Implementation Matters - PPO vs TRPO (2020)|Engstrom 2020]]). "PPO beats TRPO" is substantially "PPO-the-artifact beats TRPO-the-artifact"
- **The 250,000-agent tuning study** (50+ design choices in one framework): initialize the **last policy layer ~100× smaller** so the initial policy is near-uniform (one of the largest single effects found); normalize observations; normalize advantages per batch; Adam at ~3e-4 with annealing; the value-clipping trick, meanwhile, shows little benefit ([[What Matters in On-Policy RL (2020)|Andrychowicz 2020]])

Same lesson as [[Fantastic Pretraining Optimizers II - Hyperball (2026)|Hyperball]] and [[On Bonus Based Exploration Methods - Taiga (2020)|Taiga]], now for the third domain: **the named idea explains less of the performance than the engineering around it.** When a PPO run fails, suspect this list before the objective.

## PPO in LLM training — and why this branch won

[[RLHF]] (the InstructGPT recipe) is this note applied to the token MDP: per-**token** ratios $\pi_\theta(y_t \mid x, y_{<t}) / \pi_{\text{old}}(\cdot)$, [[Generalized Advantage Estimation|GAE]] advantages from a value head, the clip doing exactly its usual job. One notable addition, easy to confuse with the trust region: an extra **KL penalty against the frozen reference model** (the SFT checkpoint). Different object, different purpose — the *clip* keeps $\pi_\theta$ near the recent $\pi_{\text{old}}$ for estimator validity (resets every batch); the *reference-KL* keeps it near the original model to prevent reward hacking and preserve capabilities (never resets). Details → [[RLHF]].

Why the LLM world runs on this and not on the [[Deep Q-Network|value-based branch]] — the full argument, assembled from the whole cluster: the pretrained LM already **is** a policy (no argmax over 100k tokens needed); on-policy training drops the [[Temporal Difference Learning#The deadly triad|deadly triad's]] third leg outright; the update is a weighted cross-entropy — infrastructure LLM training already has; and sample cost, PG's classic weakness, inverts at scale — generating rollouts is cheap **parallel inference**, while the stability PPO buys is exactly what training a fragile 100B-parameter policy demands.

## Related

- [[Policy Gradient]] → [[Actor-Critic]] → [[Generalized Advantage Estimation]] — the chain this completes
- [[Exploration vs Exploitation]] — the entropy term's ancestry
- [[RLHF]] · [[GRPO]] — the descendants
- [[Deep Q-Network]] — the other branch's answer to sample reuse (replay), for contrast

## Sources

- [[Trust Region Policy Optimization (2015)]] — the theory and the price
- [[Proximal Policy Optimization (2017)]] — the clip
- [[High-Dimensional Continuous Control Using GAE (2015)]] — the advantage estimator inside
- [[Implementation Matters - PPO vs TRPO (2020)]] — code-level attribution
- [[What Matters in On-Policy RL (2020)]] — the 250k-agent tuning evidence

---
Part of the policy-gradient branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
