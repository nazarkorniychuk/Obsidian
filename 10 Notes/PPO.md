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

So the goal is precise: **squeeze several epochs of updates out of each batch (fix 1), while guaranteeing the policy never moves far from the one that collected the data (fix 2).** Two tools are needed first — one to make old data usable at all, one to measure "far."

## Ingredient 1: importance sampling — how old data becomes usable

**The question:** the batch was collected by $\pi_{old}$; how good would a *candidate* policy be? Tiny concrete case — one state, two actions, advantages measured from the data:

| | $\pi_{old}$ | $\pi_{new}$ | advantage |
|---|---|---|---|
| Left | 0.5 | 0.8 | **+1** |
| Right | 0.5 | 0.2 | **−1** |

What we want (the new policy's expected advantage): $0.8(+1) + 0.2(-1) = \mathbf{+0.6}$. What the batch naively gives: it's ~50/50 Left/Right (that's how it was collected), so a plain average says $0.5(+1) + 0.5(-1) = 0$ — the *old* policy's number, useless for judging the new one.

**The fix: reweight each sample by how much more the new policy "would have" produced it** — the **ratio** $r = \pi_{new}(a)/\pi_{old}(a)$. Left samples: $r = 0.8/0.5 = 1.6$, each counts 1.6×. Right: $r = 0.4$, each counts 0.4×. Reweighted average: $0.5(1.6)(+1) + 0.5(0.4)(-1) = \mathbf{+0.6}$ — exactly right, from old data alone. Why it's exact, in one line — the sample frequency cancels against the ratio's denominator:

$$\sum_a \underbrace{\pi_{old}(a)}_{\text{how often it's in the data}} \cdot \frac{\pi_{new}(a)}{\pi_{old}(a)} \cdot A(a) \;=\; \sum_a \pi_{new}(a)\, A(a)$$

Everyday version: **survey reweighting** — you polled 50/50 men/women but the population is 80/20, so you weight each respondent by (population share)/(sample share) and the poll speaks for the population. The batch is a poll conducted under the old policy; ratios make it speak for the new one.

**Where it breaks — the reason everything after exists.** Let the new policy love an action the old one almost never tried: $\pi_{new} = 0.5$, $\pi_{old} = 0.01$ → ratio **50**. The whole estimate now hangs on a couple of lucky samples screaming with weight 50 — unbiased in theory, noise in practice. If the old policy took it *zero* times, no weight can conjure the missing information ([[Exploration vs Exploitation|coverage]] again). And the *states* in the batch are the old policy's states regardless — the ratios only fix the actions. **Conclusion: the reweighted estimate is exact in expectation but only *trustworthy* near the old policy.** Which demands a ruler for "near."

## Ingredient 2: KL divergence — the ruler for "how far did the policy move"

$$\text{KL}(p \,\|\, q) = \sum_x p(x)\, \log\frac{p(x)}{q(x)} \qquad \text{— the average log-ratio between two distributions}$$

Zero iff identical; grows with divergence. Numbers, using the policies above: old (0.5, 0.5) vs new (0.8, 0.2) → KL ≈ **0.22 nats**; vs a gentle (0.55, 0.45) → KL ≈ **0.005** — 40× smaller for a 10× smaller shift. KL grows *quadratically* near zero: tiny behavioral changes are nearly free, large ones expensive — the right shape for a step-size ruler.

Two reasons it's *the* ruler here and not distance in θ:

1. **θ-distance measures the wrong thing.** Probabilities are exponentials of logits — a small θ nudge can massively move the action distribution, a large one can barely move it. KL measures what matters: how differently the policies *behave*
2. **KL is literally the expected log of the importance ratio.** Bounding KL directly bounds how wild the ratios from Ingredient 1 get — it is precisely a cap on how untrustworthy the reweighted estimate may become

One sentence to keep: **importance ratios are how you *spend* old data; KL is how you *measure* whether you've spent too much.**

## First answer: TRPO — the trust region, done rigorously

The theory rests on three stacked results ([[Trust Region Policy Optimization (2015)|Schulman 2015]]); walking through them explains *exactly* where the danger of stale data lives.

**Step 1 — the exact starting point: the performance difference lemma.** How much better is a candidate policy $\pi$ than the current $\pi_{old}$? There's an exact identity (Kakade & Langford):

$$J(\pi) - J(\pi_{old}) \;=\; \mathbb{E}_{\,s \sim d^{\pi},\; a \sim \pi}\big[\, A^{\pi_{old}}(s, a)\,\big]$$

In words: **the new policy's total gain = the old policy's advantages, averaged over the states and actions the *new* policy visits.** Beautiful — improvement is exactly "how often does the new policy take actions the old policy's own judgment rates above average." One fatal flaw: the expectation runs over $d^{\pi}$, the state distribution of a policy **you haven't run yet**. Unsamplable.

**Step 2 — two substitutions build the surrogate.** Replace both unsamplable pieces with samplable ones:

- *States:* $d^{\pi} \to d^{\pi_{old}}$ — use the states in the batch you already collected. This is an *approximation*, and it is precisely the "the policy generates its own next dataset" problem in formal dress: it ignores how the state distribution will shift when π changes
- *Actions:* rewrite $\mathbb{E}_{a \sim \pi}[\cdot]$ using old actions via **importance sampling — Ingredient 1, applied verbatim**: reweight each sample by $r_t(\theta) = \tfrac{\pi_\theta(a_t \mid s_t)}{\pi_{old}(a_t \mid s_t)}$. This part is *exact* (the survey-reweighting identity)

The result is the **surrogate objective**, computable entirely from the old batch:

$$L_{\pi_{old}}(\theta) = \mathbb{E}_{\,s, a \,\sim\, \pi_{old}}\big[\, r_t(\theta)\, \hat{A}_t\,\big]$$

It matches the true $J$ **to first order** at $\theta = \theta_{old}$ (same value, same gradient — differentiate the ratio and check: at $\pi = \pi_{old}$, $r = 1$ and $\nabla r = \nabla \log \pi$, recovering the [[Policy Gradient|policy gradient]]). So near the old policy the surrogate is trustworthy; far away, the state-substitution error takes over.

**Step 3 — bound the error, get a guarantee.** TRPO's main theorem quantifies "far away": the truth is sandwiched by the surrogate minus a penalty proportional to how much the policies disagree,

$$J(\pi) \;\ge\; L_{\pi_{old}}(\pi) \;-\; C \cdot \max_s \text{KL}\big(\pi_{old} \,\|\, \pi\big)(s), \qquad C = \frac{4\,\epsilon\,\gamma}{(1-\gamma)^2}$$

(ε = the largest advantage magnitude; note the $\tfrac{1}{(1-\gamma)^2}$ — the [[Markov Decision Process|horizon tax]] again, because state-distribution shift compounds over the horizon.) This turns improvement into a **minorization-maximization** argument: the right side is a *lower bound* that touches $J$ at $\pi_{old}$ — so any π that raises the lower bound must raise the truth by at least as much. **Guaranteed monotonic improvement**, in the same family of arguments as [[Bellman Equation|policy iteration's]] improvement theorem.

**Step 4 — from theorem to algorithm (and where the price appears).** The theoretical penalty $C$ is astronomically conservative (steps would be microscopic), so practice swaps the penalty for a **hard constraint with a fixed budget**: maximize $L$ subject to $\overline{\text{KL}}(\pi_{old} \| \pi_\theta) \le \delta$ (mean KL, not max). Solving *that* is what costs: expand the KL to second order (its Hessian is the **Fisher information matrix** — this is natural gradient territory), solve the resulting quadratic-constrained linear problem by **conjugate gradient**, then **line-search** back until the true constraint and improvement both hold. Second-order machinery, extra passes, and incompatibility with parameter sharing (policy+value in one network) and everyday architecture choices.

**Results:** robust learning of swimming/hopping/walking gaits and Atari **with little hyperparameter tuning** — the reliability that made trust regions standard. **The verdict: right idea, wrong price** — and note what the idea actually was: *measure step size where it matters (KL in policy space, not norms in parameter space), and buy improvement guarantees by staying inside the region where your stale-data estimate is trustworthy.* PPO keeps exactly that idea and throws away the machinery.

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
