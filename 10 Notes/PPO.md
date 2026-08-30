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

## The derivation: building a reusable objective

**Destination, stated first.** From a batch collected by $\pi_{old}$, build the surrogate

$$\boxed{\;L(\theta) = \mathbb{E}_{\,s,a \sim \pi_{old}}\big[\,r(\theta)\,\hat{A}\,\big], \qquad r(\theta) = \frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}\;}$$

**What every symbol is, before anything else.** The batch = trajectories rolled out by $\pi_{old}$. For each $(s, a)$ in it, $\hat{A}$ is the advantage estimated **entirely under the old policy**: the action was taken by $\pi_{old}$, and it's judged against the old critic $V_\phi$ (trained on $\pi_{old}$'s data), combined over the old trajectory's TD errors by [[Generalized Advantage Estimation|GAE]]. So $\hat{A} = \hat{A}^{\pi_{old}}(s,a)$ — *"how much this action beat the __old__ policy's expectations."* Crucially, these are **frozen numbers**: computed once when the batch is collected, never touched again during the epochs of optimization. In $L(\theta)$, the *only* thing that depends on θ is the ratio $r(\theta)$; the $\hat{A}$'s are constants riding along.

We'll establish two facts about $L$: **(P1)** at $\theta_{old}$ its gradient equals the true policy gradient — so climbing it starts out correct; **(P2)** the truth is never worse than $J(\theta_{old}) + L(\theta) - C\cdot\text{KL}(\pi_{old}\|\pi_\theta)$ — so climbing it *inside a KL fence* is guaranteed progress. TRPO enforces the fence exactly (expensive); PPO fakes it with a clip (cheap). Five steps.

---

**Step 1 — what $L$ is: replay the batch, regrade the actions.**

In words first: go through every $(s, a, \hat{A})$ in the batch and multiply its advantage by *how much more (or less) the candidate policy would have chosen that action* — the ratio $r$. Sum up. That's $L$.

Why that's the right recipe: at any single state, "the average advantage of the actions $\pi_\theta$ would pick" can be rewritten to use only $\pi_{old}$'s samples — multiply and divide by $\pi_{old}$:

$$\mathbb{E}_{a \sim \pi_\theta}\big[\hat{A}\big] \;=\; \sum_a \pi_\theta(a \mid s)\,\hat{A} \;=\; \sum_a \pi_{old}(a \mid s)\cdot\underbrace{\frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}}_{r(\theta)}\cdot\hat{A} \;=\; \mathbb{E}_{a \sim \pi_{old}}\big[\,r(\theta)\,\hat{A}\,\big]$$

Numeric check — $\pi_{old} = (.5, .5)$, $\pi_\theta = (.8, .2)$, $\hat{A} = (+1, -1)$: direct answer $0.8(+1) + 0.2(-1) = \mathbf{+0.6}$; reweighted batch $0.5(1.6)(+1) + 0.5(0.4)(-1) = \mathbf{+0.6}$ ✓.

What $L$ *measures*: the estimated **gain over the old policy**, not absolute performance. Sanity check at the anchor: $L(\theta_{old}) = \mathbb{E}_{a\sim\pi_{old}}[\hat{A}] = 0$, because advantages are zero-mean under their own policy ([[Bellman Equation|A = Q − V]]) — the gain of not moving is zero, as it should be. Since $J(\theta_{old})$ is a fixed constant, maximizing the gain is the same as maximizing $J$.

**Step 2 — P1: the slope of $L$ at the start is the true policy gradient.** Slowly, three small motions.

*Motion 1 — differentiate the ratio.* In $r(\theta) = \frac{\pi_\theta(a|s)}{\pi_{old}(a|s)}$ the denominator is just a **number** (it has no θ in it), so:

$$\nabla_\theta\, r(\theta) = \frac{\nabla_\theta\, \pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}$$

*Motion 2 — evaluate at the anchor.* At $\theta = \theta_{old}$ the policies coincide, $\pi_{old} = \pi_{\theta}$, so the denominator can be renamed:

$$\nabla_\theta\, r\,\Big|_{\theta_{old}} = \frac{\nabla_\theta\, \pi_\theta}{\pi_\theta}\Bigg|_{\theta_{old}} = \nabla_\theta \log \pi_\theta(a \mid s)\,\Big|_{\theta_{old}}$$

(last equality = the log-derivative identity $\nabla \log f = \nabla f / f$, the same one that powers [[Policy Gradient|REINFORCE]]).

*Motion 3 — plug into $L$.* Since $L = \mathbb{E}[r\hat{A}]$ and $\hat{A}$ doesn't depend on θ:

$$\nabla_\theta L\,\Big|_{\theta_{old}} = \mathbb{E}_{\,s,a\sim\pi_{old}}\big[\,\hat{A}\; \nabla_\theta \log \pi_\theta(a \mid s)\,\big]\Big|_{\theta_{old}}$$

Compare with the [[Policy Gradient|policy gradient theorem]]: this **is** $\nabla_\theta J\,\big|_{\theta_{old}}$, term for term. Conclusion: **the first SGD step on $L$ is exactly a true policy-gradient step.** $L$ starts out pointing the right way; the only question is how long it *keeps* pointing the right way.

**Step 3 — where $L$ goes wrong as θ moves away.** Write both gradients at a *general* θ and compare piece by piece:

| piece | in $\nabla L(\theta)$ | in $\nabla J(\theta)$ | match? |
|---|---|---|---|
| actions | $a \sim \pi_\theta$ (the ratio's doing) | $a \sim \pi_\theta$ | ✅ at every θ |
| states | the batch's states ($\pi_{old}$'s) | the *new* policy's states | ❌ |
| advantages | $\hat{A}^{\pi_{old}}$ — old judgments | $A^{\pi_\theta}$ | ❌ |

So the equality of Step 2 is **true only at the anchor** — away from it, two cracks open and widen with distance:

- **Crack 1 — ratio variance:** $\pi_{old}(a) = 0.01,\ \pi_\theta(a) = 0.5 \Rightarrow r = 50$: a few lucky samples, weighted 50×, carry the whole estimate
- **Crack 2 — frozen states:** the new policy will visit places the batch never saw, and $L$ knows nothing about them; no ratio can fix this one

**Step 4 — measure "distance" the right way: KL.**

$$\text{KL}(p \,\|\, q) = \sum_x p(x)\,\log\frac{p(x)}{q(x)} \qquad — \;0 \text{ iff } p = q;\; \text{grows with divergence}$$

Numbers: $(.5,.5)$ vs $(.8,.2)$ → 0.22 nats; vs $(.55,.45)$ → 0.005. Why KL and not $\|\theta - \theta_{old}\|$: behavior, not parameters, is what drifts ([[Attention Mechanism|softmax]] of logits — tiny θ moves can be huge distribution moves). And KL is tailor-made for Crack 1: $\text{KL}(\pi_{old}\|\pi_\theta) = \mathbb{E}_{\pi_{old}}[-\log r]$ — **it is the average log of the very ratio that misbehaves**, so capping KL caps the ratios.

**Step 5 — P2: the guarantee, and TRPO.** The rigorous analysis (Kakade & Langford's performance difference lemma + [[Trust Region Policy Optimization (2015)|Schulman 2015]]) prices both cracks in KL:

$$J(\theta) \;\ge\; J(\theta_{old}) + L(\theta) \;-\; C\,\max_s \text{KL}\big(\pi_{old} \| \pi_\theta\big)(s), \qquad C = \tfrac{4\epsilon\gamma}{(1-\gamma)^2}$$

The improvement argument, in three short sentences: **(i)** the right side never exceeds $J$ — it's a *floor*. **(ii)** At $\theta_{old}$ the floor *touches*: both sides equal $J(\theta_{old})$ (since $L = 0$, KL $= 0$ there). **(iii)** Therefore, if you find any θ where the floor is higher than $J(\theta_{old})$, the truth $J(\theta)$ — sitting on or above the floor — must be higher too. **Guaranteed improvement**, and note it needed *nothing* about $L$'s gradient away from the anchor.

TRPO turns this into an algorithm by swapping the (hugely conservative) penalty for a hard budget: $\max_\theta \mathbb{E}[r\hat{A}]$ s.t. $\overline{\text{KL}} \le \delta$ — solved with second-order machinery (Fisher matrix, conjugate gradient, line search). **Results:** robust gaits + Atari with little tuning. **Price:** complex, expensive, incompatible with shared policy/value networks. **PPO keeps Steps 1–5 and replaces only the solver.**

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
