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

So the goal is precise: **squeeze several epochs of updates out of each batch (fix 1), while guaranteeing the policy never moves far from the one that collected the data (fix 2).** The rest is one derivation, done move by move.

## The derivation: policy gradient → TRPO, move by move

**Start — what we have, and why it's single-use.** The on-policy gradient from [[Policy Gradient]]:

$$\nabla_\theta J = \mathbb{E}_{\,s \sim d^{\pi_\theta},\; a \sim \pi_\theta}\big[\,\hat{A}\; \nabla_\theta \log \pi_\theta(a \mid s)\,\big]$$

Both expectations are under the *current* $\pi_\theta$ — one gradient step later, the batch no longer qualifies. Goal: an objective $L(\theta)$ computable from a $\pi_{old}$-batch, optimizable for many steps.

**Move 1 — exact rewrite (performance difference lemma):**

$$J(\theta) - J(\theta_{old}) \;=\; \mathbb{E}_{\,s \sim d^{\pi_\theta}}\,\mathbb{E}_{\,a \sim \pi_\theta}\big[\, A^{\pi_{old}}(s, a)\,\big]$$

New policy's gain = old policy's advantages under the **new** policy's state visitation $d^{\pi_\theta}$. Exact — but $d^{\pi_\theta}$ belongs to a policy not yet run. Two unsamplable pieces; one move each.

**Move 2 — replace the states (the approximation):**

$$d^{\pi_\theta} \;\longrightarrow\; d^{\pi_{old}}$$

Use the batch's states as-is. This ignores how visitation shifts with θ; the error is billed in Move 5.

**Move 3 — replace the action distribution (exact — importance sampling):**

$$\mathbb{E}_{a \sim \pi_\theta}[A] \;=\; \sum_a \pi_\theta(a \mid s)\,A \;=\; \sum_a \pi_{old}(a \mid s)\,\frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}\,A \;=\; \mathbb{E}_{a \sim \pi_{old}}\big[\,r(\theta)\,A\,\big], \qquad r(\theta) = \frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}$$

Numeric check — $\pi_{old} = (.5, .5)$, $\pi_\theta = (.8, .2)$, $A = (+1, -1)$: target $0.8 - 0.2 = \mathbf{+0.6}$; reweighted batch $0.5(1.6)(+1) + 0.5(0.4)(-1) = \mathbf{+0.6}$ ✓. Failure mode: $\pi_{old}(a) = 0.01,\ \pi_\theta(a) = 0.5 \Rightarrow r = 50$ — a couple of lucky samples carry the whole estimate: exact in expectation, variance unbounded. The quantity to control: how far ratios stray from 1.

**Result of Moves 1–3 — the surrogate:**

$$L_{\theta_{old}}(\theta) \;=\; \mathbb{E}_{\,s, a \sim \pi_{old}}\big[\, r(\theta)\, \hat{A}\,\big]$$

**Move 4 — sanity: $L$ matches $J$ to first order.** At $\theta = \theta_{old}$: $r \equiv 1$, and

$$\nabla_\theta\, r\,\big|_{\theta_{old}} = \frac{\nabla_\theta \pi_\theta}{\pi_{old}}\bigg|_{\theta_{old}} = \nabla_\theta \log \pi_\theta \quad\Longrightarrow\quad \nabla_\theta L\,\big|_{\theta_{old}} = \mathbb{E}\big[\hat{A}\,\nabla_\theta\log \pi_\theta\big] = \nabla_\theta J\,\big|_{\theta_{old}}$$

Same value, same gradient — a legitimate local stand-in. Only the *step size* remains in question.

**Move 5 — bill the Move-2 error (the bound).** Distance is measured by KL, $\text{KL}(p \| q) = \sum_x p(x)\log\tfrac{p(x)}{q(x)}$, and the choice is not arbitrary:

$$\text{KL}\big(\pi_{old} \,\|\, \pi_\theta\big) = \mathbb{E}_{\,a \sim \pi_{old}}\big[-\log r(\theta)\big]$$

— **KL is the expected negative log of the Move-3 ratio**: bounding it bounds exactly the failure mode above. (Scale: $(.5,.5)$ vs $(.8,.2)$ → 0.22 nats; vs $(.55,.45)$ → 0.005 — quadratic near zero.) TRPO's theorem:

$$J(\theta) \;\ge\; L_{\theta_{old}}(\theta) \;-\; C\,\max_s \text{KL}\big(\pi_{old} \| \pi_\theta\big)(s), \qquad C = \frac{4\,\epsilon\,\gamma}{(1-\gamma)^2}$$

The RHS is a lower bound that **equals $J$ at $\theta_{old}$** — so any θ raising the RHS raises $J$ by at least as much: **monotonic improvement** (same argument family as [[Bellman Equation|policy iteration's]] improvement theorem; the $(1{-}\gamma)^{-2}$ is the [[Markov Decision Process|horizon tax]] on distribution shift).

**Move 6 — the algorithm.** The theoretical $C$ forces microscopic steps, so practice swaps penalty → hard constraint:

$$\max_\theta\; \mathbb{E}\big[\,r(\theta)\,\hat{A}\,\big] \quad \text{s.t.} \quad \mathbb{E}_s\big[\text{KL}(\pi_{old} \| \pi_\theta)\big] \le \delta$$

Solved to second order: $\text{KL} \approx \tfrac{1}{2}\Delta\theta^{\top} F\, \Delta\theta$ with $F$ the Fisher information matrix → the natural-gradient step $\Delta\theta \propto F^{-1} \nabla L$ via conjugate gradient, plus a line search re-verifying the true constraint. **Results:** robust gaits + Atari with little tuning ([[Trust Region Policy Optimization (2015)|Schulman 2015]]). **The catch:** second-order machinery — expensive, complex, incompatible with shared policy/value networks. Right objective, wrong solver. PPO keeps Moves 1–5 and replaces only Move 6.

## PPO: the trust region as a loss function

One substitution ([[Proximal Policy Optimization (2017)|Schulman 2017]]):

$$\max_\theta\, \mathbb{E}\big[r\hat{A}\big] \;\text{ s.t. }\; \overline{\text{KL}} \le \delta \qquad\longrightarrow\qquad \max_\theta\, \mathbb{E}\Big[\min\big(r\hat{A},\; \text{clip}(r, 1{-}\epsilon, 1{+}\epsilon)\,\hat{A}\big)\Big]$$

The constrained second-order problem becomes an unconstrained first-order loss:

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

[[RLHF]] (the InstructGPT recipe) is this note applied to the token MDP: per-**token** ratios $\pi_\theta(y_t \mid x, y_{<t}) / \pi_{\text{old}}(\cdot)$, [[Generalized Advantage Estimation|GAE]] advantages from a value head, the clip doing exactly its usual job. One notable addition, easy to confuse with the trust region: an extra **KL penalty against the frozen reference model** (the SFT checkpoint). All the leashes in one table — each is a KL-flavored constraint, but to a *different anchor* for a *different reason*:

| leash | keeps $\pi_\theta$ close to | purpose | resets? |
|---|---|---|---|
| TRPO's constraint $\overline{KL} \le \delta$ | $\pi_{old}$ (last batch's policy) | keep the importance-sampled estimate trustworthy | every batch |
| PPO's clip on $r \in [1{-}\epsilon, 1{+}\epsilon]$ | $\pi_{old}$ | same goal, enforced per-sample on the ratio itself — the cheap proxy | every batch |
| RLHF's $\beta\,\text{KL}(\pi_\theta \| \pi_{ref})$ | the frozen SFT reference | prevent [[Reward Model|reward hacking]], preserve the pretrained distribution | **never** |

Details → [[RLHF]].

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
