---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [Proximal Policy Optimization, PPO-clip, clipped surrogate objective, TRPO]
---

# PPO

> **Where this sits.** The workhorse of the [[Policy Gradient|policy-gradient]] branch and the algorithm of [[RLHF]]. Its entire content is a safe answer to one question: **how do you take more than one gradient step on the same batch of on-policy data without destroying the policy?** This note also contains TRPO in full — PPO is TRPO with the expensive part swapped out, so they share most of their story.

## The map of this note

| part | its question | its answer |
|---|---|---|
| Baseline & problem | what's wrong with the vanilla loop? | one update per batch, and oversized steps self-poison |
| **Part 1 — the surrogate $L$** | how can *old* data grade a *new* policy? | the importance ratio: $L = \mathbb{E}[r\,\hat{A}^{\pi_{old}}]$ — exact at the anchor, cracks with distance |
| **Part 2 — TRPO** | how far from the anchor is safe? | inside a KL ball of radius δ — enforced *exactly*, with second-order machinery |
| **Part 3 — PPO** | can the ball be enforced cheaply? | yes — clip the ratio *inside the loss*, then just run Adam |

Each part opens with its destination, then earns it. Parts 2 and 3 are two enforcement mechanisms for the *same* fence — that's the one sentence to hold onto.

## The baseline: the loop both algorithms improve on

Everything in this note modifies one baseline — the basic on-policy loop from [[Policy Gradient]] + [[Actor-Critic]]. State it fully so the changes have something to be changes *to*:

1. **Roll out** the current policy $\pi_{\theta_{old}}$ → a batch of transitions
2. **Grade it**: compute $\hat{A}^{\pi_{old}}(s,a)$ for every pair in the batch (critic $V_\phi$ + [[Generalized Advantage Estimation|GAE]])
3. **One gradient step** on the policy:

$$\theta \;\leftarrow\; \theta_{old} + \alpha\; \mathbb{E}_{\,s,a \sim \pi_{old}}\big[\,\hat{A}^{\pi_{old}}\, \nabla_\theta \log \pi_\theta(a \mid s)\,\big]\Big|_{\theta_{old}}$$

4. **Throw the batch away.** Return to 1.

Equivalently, step 3 is one SGD step on the loss $L_{PG}(\theta) = \mathbb{E}\big[\log \pi_\theta(a \mid s)\cdot \hat{A}^{\pi_{old}}\big]$ — a *cross-entropy weighted by advantages*: push up the log-prob of actions that beat expectations, push down the rest. This works ([[Asynchronous Methods for Deep RL - A3C (2016)|A3C]] is exactly this plus parallelism). PPO exists because of what's wrong with steps 3–4.

## The problem: single-use data, and steps that self-poison

Two compounding pains of that loop:

**1. One update per sample.** The policy gradient is an expectation under the *current* policy. After a single gradient step, θ has moved — the batch you just collected is now another policy's data, and the estimator is no longer valid. Strictly, you collect → step once → throw everything away. Given that collecting is the expensive part (weeks of simulated time — [[High-Dimensional Continuous Control Using GAE (2015)|the GAE ledger]]), single-use data is brutal.

**2. A too-big step is not just slow to fix — it's self-poisoning.** In supervised learning a bad step is fine: the dataset is fixed, the next steps walk back. In on-policy RL **the policy generates the next dataset.** Step too far → policy degrades → *it collects degraded data* → gradients computed on garbage → further degradation. A death spiral, from one oversized step. Worse, the danger is invisible in parameter space: the gradient is a local approximation, and a small change in θ can be a *huge* change in the action distribution (probabilities are exponentials of logits). Step size in θ is the wrong dial; what needs limiting is the step in *policy space*.

So the goal is precise: **squeeze several epochs of updates out of each batch (fix 1), while guaranteeing the policy never moves far from the one that collected the data (fix 2).**

---

## Part 1 — The surrogate objective L: old data grades a new policy

### The destination

From a batch collected by $\pi_{old}$, build the surrogate

$$\boxed{\;L(\theta) = \mathbb{E}_{\,s,a \sim \pi_{old}}\big[\,r(\theta)\,\hat{A}^{\pi_{old}}\,\big], \qquad r(\theta) = \frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}\;}$$

whose gradient (derived in Step 2) is the ordinary policy gradient with an **importance weight riding on every sample**:

$$\boxed{\;\nabla_\theta L(\theta) = \mathbb{E}_{\,s,a \sim \pi_{old}}\big[\, r(\theta)\; \hat{A}^{\pi_{old}}\; \nabla_\theta \log \pi_\theta(a \mid s)\,\big]\;}$$

**What every symbol is, before anything else.** The batch = trajectories rolled out by $\pi_{old}$. For each $(s, a)$ in it, $\hat{A}^{\pi_{old}}$ is the advantage estimated **entirely under the old policy**: the action was taken by $\pi_{old}$, and it's judged against the old critic $V_\phi$ (trained on $\pi_{old}$'s data), combined over the old trajectory's TD errors by [[Generalized Advantage Estimation|GAE]]. The superscript keeps the ownership visible in the symbol itself: $\hat{A}^{\pi_{old}}(s,a)$ = *"how much this action beat the __old__ policy's expectations."* Crucially, these are **frozen numbers**: computed once when the batch is collected, never touched again during the epochs of optimization. In $L(\theta)$, the *only* thing that depends on θ is the ratio $r(\theta)$; the $\hat{A}^{\pi_{old}}$'s are constants riding along.

This part establishes one fact and one warning: **(P1)** at $\theta_{old}$, the gradient of $L$ equals the true policy gradient — so climbing $L$ starts out exactly correct; **(warning, Step 3)** away from $\theta_{old}$, two cracks open between $L$ and the truth, and they widen with distance. What to *do* about the cracks is Parts 2 and 3.

### Step 1 — what L is: replay the batch, regrade the actions

In words first: go through every $(s, a, \hat{A}^{\pi_{old}})$ in the batch and multiply its advantage by *how much more (or less) the candidate policy would have chosen that action* — the ratio $r$. Sum up. That's $L$.

Why that's the right recipe: at any single state, "the average advantage of the actions $\pi_\theta$ would pick" can be rewritten to use only $\pi_{old}$'s samples — multiply and divide by $\pi_{old}$:

$$\mathbb{E}_{a \sim \pi_\theta}\big[\hat{A}^{\pi_{old}}\big] \;=\; \sum_a \pi_\theta(a \mid s)\,\hat{A}^{\pi_{old}} \;=\; \sum_a \pi_{old}(a \mid s)\cdot\underbrace{\frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}}_{r(\theta)}\cdot\hat{A}^{\pi_{old}} \;=\; \mathbb{E}_{a \sim \pi_{old}}\big[\,r(\theta)\,\hat{A}^{\pi_{old}}\,\big]$$

What $L$ *measures*: the estimated **gain over the old policy**, not absolute performance. Sanity check at the anchor: $L(\theta_{old}) = \mathbb{E}_{a\sim\pi_{old}}[\hat{A}^{\pi_{old}}] = 0$, because advantages are zero-mean under their own policy ([[Bellman Equation|A = Q − V]]) — the gain of not moving is zero, as it should be. Since $J(\theta_{old})$ is a fixed constant, maximizing the gain is the same as maximizing $J$.

### Step 2 — the gradient of L, derived, then evaluated at the anchor

*Motion 1 — the gradient passes through the expectation.* This is the structural payoff of the whole construction, so notice it: in $J(\theta)$, the θ sits in the *sampling distribution* — you cannot push $\nabla_\theta$ inside (that's why [[Policy Gradient]] needed the log-derivative trick). In $L(\theta)$, the sampling distribution is $\pi_{old}$ — **θ-free** — and all θ-dependence has been moved into the integrand (the ratio). So the gradient slides straight in:

$$\nabla_\theta L(\theta) = \nabla_\theta\, \mathbb{E}_{\,s,a \sim \pi_{old}}\big[\, r(\theta)\, \hat{A}^{\pi_{old}}\big] = \mathbb{E}_{\,s,a \sim \pi_{old}}\big[\, \nabla_\theta r(\theta)\cdot \hat{A}^{\pi_{old}}\big]$$

(the $\hat{A}^{\pi_{old}}$ are frozen constants — they pass through untouched).

*Motion 2 — differentiate the ratio.* In $r(\theta) = \frac{\pi_\theta(a|s)}{\pi_{old}(a|s)}$ the denominator is just a **number** (no θ in it), so:

$$\nabla_\theta\, r(\theta) = \frac{\nabla_\theta\, \pi_\theta(a \mid s)}{\pi_{old}(a \mid s)} \;=\; \underbrace{\frac{\pi_\theta(a \mid s)}{\pi_{old}(a \mid s)}}_{r(\theta)}\cdot \underbrace{\frac{\nabla_\theta \pi_\theta(a \mid s)}{\pi_\theta(a \mid s)}}_{\nabla_\theta \log \pi_\theta}$$

(second form: multiply and divide by $\pi_\theta$, then apply the log-derivative identity $\nabla \log f = \nabla f / f$ — REINFORCE's own trick). Substituting back gives the boxed general gradient from the destination — the ordinary policy gradient with the weight $r(\theta)$ riding on every sample. This is literally the gradient your PPO code computes at each inner epoch (before clipping).

*Motion 3 — evaluate at the anchor (P1).* At $\theta = \theta_{old}$: $r \equiv 1$, and the weight disappears:

$$\nabla_\theta L\,\Big|_{\theta_{old}} = \mathbb{E}_{\,s,a\sim\pi_{old}}\big[\,\hat{A}^{\pi_{old}}\; \nabla_\theta \log \pi_\theta(a \mid s)\,\big]\Big|_{\theta_{old}} \;=\; \nabla_\theta J\,\Big|_{\theta_{old}}$$

— term for term the [[Policy Gradient|policy gradient theorem]]. Conclusion: **the first SGD step on $L$ is exactly a true policy-gradient step.** $L$ starts out pointing the right way; the only question is how long it *keeps* pointing the right way as the epochs move θ and the weight $r(\theta)$ drifts from 1.

### Step 3 — where L cracks as θ moves away

Take the boxed general gradient (note $\pi_{old} \cdot r(\theta) = \pi_\theta$, so its action-average is effectively under $\pi_\theta$), set it beside the *true* gradient at that θ, and compare piece by piece:

| piece | in $\nabla L(\theta)$ | in $\nabla J(\theta)$ | match? |
|---|---|---|---|
| actions | $a \sim \pi_\theta$ (the ratio's doing) | $a \sim \pi_\theta$ | ✅ at every θ |
| states | the batch's states ($\pi_{old}$'s) | the *new* policy's states | ❌ |
| advantages | $\hat{A}^{\pi_{old}}$ — old judgments | $A^{\pi_\theta}$ | ❌ |

So the equality of Step 2 is **true only at the anchor** — away from it, two cracks open and widen with distance:

- **Crack 1 — ratio variance:** wherever $\pi_\theta$ assigns an action much more probability than $\pi_{old}$ did, that sample's ratio is huge — a handful of such samples dominate the whole average, and $L$ becomes a high-variance estimate hanging on a few lucky draws
- **Crack 2 — frozen states:** the new policy will visit places the batch never saw, and $L$ knows nothing about them; no ratio can fix this one

**Part 1's verdict:** $L$ is a compass that is exact where you stand, decent nearby, and a liar far away. Everything that follows is a policy for *how far to walk before you stop trusting it* — TRPO measures the safe radius and enforces it exactly; PPO builds the radius into the loss.

---

## Part 2 — TRPO: the fence, enforced exactly

**TRPO in one paragraph.** Maximize $L$, subject to staying inside a KL ball of radius δ around the old policy: that is the whole algorithm ([[Trust Region Policy Optimization (2015)|Schulman 2015]]). Three things need earning: why the ball is measured in **KL** and not in parameter distance (the ruler); why staying inside the ball *guarantees* improvement (the floor theorem); and how you actually solve a constrained optimization over millions of parameters (the solver, T1–T6 — which is also the entire cost of the method, and the reason PPO exists).

### The ruler: KL divergence

$$\text{KL}(p \,\|\, q) = \sum_x p(x)\,\log\frac{p(x)}{q(x)} \qquad — \;0 \text{ iff } p = q;\; \text{grows with divergence}$$

Why KL and not $\|\theta - \theta_{old}\|$: *behavior*, not parameters, is what drifts ([[Attention Mechanism|softmax]] of logits — tiny θ moves can be huge distribution moves, and vice versa). And KL is tailor-made for Crack 1: $\text{KL}(\pi_{old}\|\pi_\theta) = \mathbb{E}_{\pi_{old}}[-\log r]$ — **it is the average log of the very ratio that misbehaves**, so capping the KL caps the ratios.

### The guarantee: a floor that touches

The rigorous analysis (Kakade & Langford's performance difference lemma + [[Trust Region Policy Optimization (2015)|Schulman 2015]]) prices *both* cracks in units of KL:

$$J(\theta) \;\ge\; J(\theta_{old}) + L(\theta) \;-\; C\,\max_s \text{KL}\big(\pi_{old} \| \pi_\theta\big)(s), \qquad C = \tfrac{4\epsilon\gamma}{(1-\gamma)^2}$$

The improvement argument, in three short sentences: **(i)** the right side never exceeds $J(\theta)$ — it's a *floor* under the truth. **(ii)** At $\theta_{old}$ the floor *touches*: both sides equal $J(\theta_{old})$, since $L(\theta_{old}) = 0$ and KL $= 0$ there. **(iii)** Therefore, if you find any θ where the floor rises above $J(\theta_{old})$, the truth $J(\theta)$ — sitting on or above the floor — must be higher too. **Guaranteed improvement** — and note the argument needed *nothing* about $L$'s gradient away from the anchor.

### The algorithm: climb L inside a KL ball

Maximizing the penalized floor directly takes tiny steps (the theorem's $C$ is hugely conservative), so TRPO swaps the penalty for a **hard budget**, and the per-state max for the batch-measurable mean:

$$\max_\theta\; \mathbb{E}\big[\,r(\theta)\,\hat{A}^{\pi_{old}}\,\big] \quad \text{s.t.} \quad \overline{\text{KL}}(\pi_{old}\,\|\,\pi_\theta) \le \delta, \qquad \delta \approx 0.01$$

(δ here is the KL budget — a hyperparameter, not a TD error). This has no closed-form solution for a neural policy — but near the anchor, both pieces admit cheap approximations, and the approximated problem *does* solve on paper. That's the solver.

### The solver, in six moves

**T1 — replace the objective by its tangent plane.** The unknown we're solving for is the step $\Delta\theta = \theta - \theta_{old}$. The true $L$ is a complicated neural-net function of that step — intractable under a constraint. So swap it for its first-order Taylor expansion at the anchor:

$$L(\theta_{old} + \Delta\theta) \;\approx\; \underbrace{L(\theta_{old})}_{=\,0\ \text{(Part 1)}} +\; g^\top \Delta\theta, \qquad g = \nabla_\theta L\,\big|_{\theta_{old}}$$

$g$ is a single fixed vector, computed by one backward pass; $g^\top \Delta\theta = \sum_i g_i \Delta\theta_i$ is the **predicted gain from taking step $\Delta\theta$** — linear in the step (its graph is a flat tilted plane, the tangent plane to $L$'s curved landscape at the point where we stand). T2 now does the same to the constraint, and the *pair* is the point: plane-inside-bowl is a toy problem with a closed-form solution (T3) — and the trust region is exactly what keeps the toy honest, since inside a small ball every smooth function is well-approximated by its tangent.

**T2 — quadratize the constraint.** Taylor-expand the KL at the anchor. Its value there is 0, and its *gradient* there is also 0 — KL is *minimized* at equality, and you can't be below a minimum in any direction — so the first surviving term is second-order:

$$\overline{\text{KL}}(\pi_{old}\,\|\,\pi_{\theta_{old}+\Delta\theta}) \;\approx\; \tfrac{1}{2}\,\Delta\theta^\top F\,\Delta\theta, \qquad F = \mathbb{E}\big[\nabla\log\pi\;\nabla\log\pi^\top\big]$$

$F$ is the **Fisher matrix** — KL's curvature at the anchor. In words: it measures, direction by direction, *how much a small parameter move bends the distribution*. For a single parameter, KL ≈ ½FΔθ² is a parabola whose steepness F says how touchy that parameter is. The problem is now pure geometry: **maximize a plane inside an ellipsoid.**

**T3 — solve the geometric problem on paper.** Lagrange gives the direction $F^{-1}g$ — the **natural gradient** — scaled to exactly touch the ellipsoid's boundary (plug $\Delta\theta = \beta F^{-1}g$ into $\tfrac{1}{2}\Delta\theta^\top F \Delta\theta = \delta$ and solve for β):

$$\Delta\theta = \sqrt{\tfrac{2\delta}{g^\top F^{-1} g}}\;\, F^{-1} g$$

What $F^{-1}$ *does*: dividing by the curvature rescales the gradient direction-by-direction — directions where a small parameter move bends the distribution a lot get shrunk hard, directions that barely move it proceed at full speed. **The natural gradient spends the KL budget where distribution-change is cheap** — exactly "limit the step in policy space, not parameter space," made into linear algebra.

**T4 — never form F.** It's (params × params) — millions squared, unstorable. Two rescues: (i) the solve only ever needs **Fisher-vector products**, $Fv = \mathbb{E}\big[(\nabla\log\pi \cdot v)\,\nabla\log\pi\big]$ — one double-backprop each, no matrix materialized; (ii) **conjugate gradient** solves $Fx = g$ from ~10 such products (with damping $(F + \lambda I)x = g$, λ ≈ 0.1, because $F$ is ill-conditioned).

**T5 — line search, because Taylor lies.** The analytic step trusted two approximations (T1's plane, T2's ellipsoid). So: try $\theta_{old} + \Delta\theta$, then *verify on the real objects* — actual mean KL ≤ δ **and** actual surrogate $L$ improved. Fail → halve the step and retry (up to ~10 halvings); all fail → take **no step**. The guarantee is checked, not assumed.

**T6 — one update per batch, total.** All of that machinery produces exactly *one* (excellent) policy update, then the batch is discarded.

TRPO's loop, assembled — put it next to the baseline loop to see what changed:

1. Roll out $\pi_{old}$ → batch; compute $\hat{A}^{\pi_{old}}$ (GAE), exactly as before
2. One backward pass → $g$ (T1); ~10 CG iterations of Fisher-vector products → $F^{-1}g$ (T4)
3. Analytic step length (T3); line-search halvings verifying real KL and real improvement (T5)
4. **One** verified update (T6); discard batch; repeat

### The bill — and why PPO exists

**Results:** robust locomotion gaits and Atari from raw pixels with almost no per-task tuning ([[Trust Region Policy Optimization (2015)|Schulman 2015]]) — the guarantee shows up in practice as *monotonic, crash-free* learning curves. **Price:** ~10–20× the cost of a plain gradient step, all spent on one update per batch (so pain 1 — single-use data — is *not even fixed*); and the double-backprop KL machinery must see the policy network *alone* — no shared policy/value trunk, no dropout, awkward with anything architecturally modern. **PPO keeps everything up to and including the fence, and replaces only this solver.**

---

## Part 3 — PPO: the fence, built into the loss

**PPO in one paragraph.** Delete the constraint and the second-order machinery entirely. Instead, rewrite the *loss* so that the **incentive to leave the KL ball disappears** — once a sample's ratio hits the fence, its gradient switches off. Then optimize with plain Adam, for **many epochs on the same batch** ([[Proximal Policy Optimization (2017)|Schulman 2017]]). Same fence as TRPO; enforcement moves from a solver into the shape of the objective — and pain 1 finally gets fixed too, because first-order updates are cheap enough to take dozens per batch.

### The objective, built in three attempts

**Attempt 0 — climb $L$ with no fence.** Run many Adam epochs on $L = \mathbb{E}[r\hat{A}^{\pi_{old}}]$ directly. The first step is a true policy-gradient step (P1) — but nothing stops the epochs from pushing profitable ratios to 5, 50, beyond: Step 3's cracks open, the estimate decouples from reality, and the policy walks confidently off a cliff. This isn't hypothetical — in the paper's ablation, the unfenced surrogate is the *worst* variant tested, worse than not reusing data at all ([[Proximal Policy Optimization (2017)|Schulman 2017]]).

**Attempt 1 — pay rent: $L - \beta\,\overline{\text{KL}}$.** The floor theorem's penalty form, with a tunable β. This is a real PPO variant (the paper's adaptive-KL version, with β raised/lowered as measured KL over/undershoots a target) — it works, but no single β fits all phases of training, and it consistently loses to the clip in head-to-head comparisons. Keep the idea (KL pressure), lose the implementation.

**Attempt 2 — cap the ratio: $\text{clip}(r, 1{-}\epsilon, 1{+}\epsilon)\,\hat{A}^{\pi_{old}}$.** Inside the band, unchanged. Outside it, $\text{clip}(r)$ is a *constant* — the loss no longer depends on θ through this sample, so its gradient is **zero**: no incentive to push further. Almost right, but symmetric clipping has a flaw: if a *bad* action's ratio has risen above $1{+}\epsilon$ (a side-effect of shared parameters moving many probabilities at once), the capped loss goes silent *exactly where a corrective push is most needed* — the clip would shield mistakes.

**Attempt 3 — the fix: be pessimistic. Take the min.**

$$\boxed{\;L^{CLIP}(\theta) = \mathbb{E}_t\Big[\min\big(\, r_t(\theta)\,\hat{A}^{\pi_{old}}_t,\;\; \text{clip}(r_t(\theta),\, 1{-}\epsilon,\, 1{+}\epsilon)\,\hat{A}^{\pi_{old}}_t \,\big)\Big], \qquad \epsilon = 0.2\;}$$

Per sample, score the policy by **whichever of the two versions is worse for it**. Consequence: *gains* saturate at the fence (the clipped constant is the smaller one, gradient dies), but *losses* are never shielded (the unclipped, more-negative branch wins the min, gradient keeps flowing). One-sided pessimism — the incentive to leave the ball is removed; the punishment for having left it never is.

### Reading the clip: all five cases

![[ppo-clip-objective.png|560]]

| case | min picks | gradient | meaning |
|---|---|---|---|
| $\hat{A}^{\pi_{old}} > 0$, $r \le 1{+}\epsilon$ | unclipped | flows ↑ | keep raising a good action |
| $\hat{A}^{\pi_{old}} > 0$, $r > 1{+}\epsilon$ | clipped constant | **zero** | already boosted 20% — the sample goes silent |
| $\hat{A}^{\pi_{old}} < 0$, $r \ge 1{-}\epsilon$ | unclipped | flows ↓ | keep suppressing a bad action |
| $\hat{A}^{\pi_{old}} < 0$, $r < 1{-}\epsilon$ | clipped constant | **zero** | already suppressed 20% — silent |
| $\hat{A}^{\pi_{old}} < 0$, $r > 1{+}\epsilon$ | **unclipped** (it's *more negative*) | **still flows ↓** | a bad action's probability *rose* via side-effects — punishment never switches off |

Net effect across the epochs: **each sample retires itself once its ratio hits the wall**, so the update naturally runs out of fuel near the boundary — a trust region enforced by *removing incentives* rather than by solving a constrained program. That is what licenses reusing the batch for many epochs: sample reuse without a replay buffer, without leaving the on-policy regime (a batch lives a few epochs, then dies — contrast [[Deep Q-Network|DQN]]'s million-step buffer).

### The training loop, concretely (what replaces T1–T6)

1. Collect the batch with $\pi_{old}$, **caching $\log \pi_{old}(a \mid s)$ per sample**; compute GAE advantages; normalize them over the batch
2. For **3–10 epochs**: shuffle into minibatches (64–256); per minibatch, one **Adam** step (lr ≈ 3e-4, annealed) on the combined loss

$$L = -L^{CLIP} \;+\; c_1\big(V_\phi(s) - V^{targ}\big)^2 \;-\; c_2\, H\big(\pi_\theta(\cdot \mid s)\big), \qquad c_1 = 0.5,\; c_2 = 0.01$$

   — policy, value, and [[Exploration vs Exploitation|entropy bonus]] in **one loss on one (shareable) network**, the thing TRPO forbade. The ratio is recomputed each step from the cache: $r = \exp\big(\log\pi_\theta - \log\pi_{old}\big)$
3. Optional guardrail: **early-stop the epochs** if measured mean KL exceeds ~0.015
4. Discard the batch; $\pi_{old} \leftarrow \pi_\theta$; repeat

### Fine print, and results

**Fine print, honestly:** the clip zeroes *gradients* — it does not hard-bound the total KL. Ratios can still drift outside the band through shared-parameter side-effects (the "Truly PPO" critique), which is why the KL early-stop guardrail and the code-level stack below carry real weight.

**Results:** outperforms the other online policy-gradient methods on MuJoCo continuous control and Atari; overall the best balance of **sample complexity, simplicity, and wall-time** ([[Proximal Policy Optimization (2017)|Schulman 2017]]). First-order only — it runs wherever Adam runs.

---

## The implementation details that matter (more than you'd like)

Two honest-attribution studies took PPO apart:

- **The code-level optimizations are load-bearing.** PPO's reference implementation ships extras absent from the paper — value-function clipping, reward scaling, orthogonal initialization, learning-rate annealing, observation normalization. Ablations show these account for **most of PPO's cumulative-reward advantage over TRPO**, and that they — not only the clip — do much of the work of keeping successive policies close ([[Implementation Matters - PPO vs TRPO (2020)|Engstrom 2020]]). "PPO beats TRPO" is substantially "PPO-the-artifact beats TRPO-the-artifact"
- **The 250,000-agent tuning study** (50+ design choices in one framework): initialize the **last policy layer ~100× smaller** so the initial policy is near-uniform (one of the largest single effects found); normalize observations; normalize advantages per batch; Adam at ~3e-4 with annealing; the value-clipping trick, meanwhile, shows little benefit ([[What Matters in On-Policy RL (2020)|Andrychowicz 2020]])

Same lesson as [[Fantastic Pretraining Optimizers II - Hyperball (2026)|Hyperball]] and [[On Bonus Based Exploration Methods - Taiga (2020)|Taiga]], now for the third domain: **the named idea explains less of the performance than the engineering around it.** When a PPO run fails, suspect this list before the objective.

## PPO in LLM training — and why this branch won

[[RLHF]] (the InstructGPT recipe) is this note applied to the token MDP: per-**token** ratios $\pi_\theta(y_t \mid x, y_{<t}) / \pi_{old}(\cdot)$, [[Generalized Advantage Estimation|GAE]] advantages from a value head, the clip doing exactly its usual job. One notable addition, easy to confuse with the trust region: an extra **KL penalty against the frozen reference model** (the SFT checkpoint). All the leashes in one table — each is a KL-flavored constraint, but to a *different anchor* for a *different reason*:

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
