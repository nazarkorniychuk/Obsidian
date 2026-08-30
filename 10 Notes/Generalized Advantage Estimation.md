---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [GAE, advantage estimation, lambda advantage]
---

# Generalized Advantage Estimation

> **Where this sits.** The [[Policy Gradient|policy gradient]] multiplies each $\nabla \log \pi$ by *an estimate of the advantage* — and every choice of estimator is a point on a bias–variance line. GAE is the whole line, with a dial. It is [[Temporal Difference Learning|TD(λ)]] transplanted from value learning into advantage estimation.

## The question: what number do you multiply the gradient by?

For the action at time $t$, "how much better than expected was this?" can be estimated many ways, using more or less of the actually-observed future:

| estimator | formula | bias | variance |
|---|---|---|---|
| 1-step (TD error) | $\delta_t = r_{t+1} + \gamma V(s_{t+1}) - V(s_t)$ | high (leans fully on the critic) | lowest |
| n-step | $r_{t+1} + \gamma r_{t+2} + \cdots + \gamma^n V(s_{t+n}) - V(s_t)$ | ↓ as n grows | ↑ as n grows |
| Monte Carlo | $G_t - V(s_t)$ | none (baseline only) | highest |

Same trade as [[Temporal Difference Learning|MC vs TD]], one level up: trust the critic (biased while it's wrong, but quiet) or trust the sampled future (honest, but loud). Committing to one n is crude — so don't.

## The estimator, derived in three steps

**Step 1 — a lemma: every n-step advantage is a sum of TD errors.** Take the 2-step case and just add $\delta_t + \gamma\,\delta_{t+1}$:

$$\underbrace{\big[r_{t+1} + \gamma V(s_{t+1}) - V(s_t)\big]}_{\delta_t} + \gamma\underbrace{\big[r_{t+2} + \gamma V(s_{t+2}) - V(s_{t+1})\big]}_{\delta_{t+1}} = r_{t+1} + \gamma r_{t+2} + \gamma^2 V(s_{t+2}) - V(s_t)$$

— the $\gamma V(s_{t+1})$ terms **cancel**, and what remains is exactly the 2-step advantage estimator from the table. The pattern telescopes at every length: each δ hands its bootstrap term to the next δ's baseline term, so

$$\hat{A}^{(n)}_t \;=\; \sum_{l=0}^{n-1} \gamma^l\, \delta_{t+l}$$

**TD errors are the atoms of advantage estimation** — every n-step estimator is just a truncated sum of them.

**Step 2 — mix all lengths with geometric weights.** Instead of committing to one n, GAE takes the exponentially-weighted average of *all* n-step estimators (weights $(1-\lambda)\lambda^{n-1}$ sum to 1 — the same mixing as [[Temporal Difference Learning|TD(λ)'s]] λ-return):

$$\hat{A}^{GAE}_t = (1-\lambda)\Big(\hat{A}^{(1)}_t + \lambda\,\hat{A}^{(2)}_t + \lambda^2\,\hat{A}^{(3)}_t + \cdots\Big)$$

**Step 3 — count how often each δ appears.** Substitute the lemma and regroup by δ instead of by n. $\delta_{t+l}$ is contained in every estimator long enough to reach it — $\hat{A}^{(n)}$ for all $n \ge l+1$ — so its total weight is

$$(1-\lambda)\big(\lambda^{l} + \lambda^{l+1} + \lambda^{l+2} + \cdots\big) = (1-\lambda)\cdot\frac{\lambda^l}{1-\lambda} = \lambda^l$$

The geometric series collapses the mixture, and with each δ's own $\gamma^l$ attached ([[High-Dimensional Continuous Control Using GAE (2015)|Schulman 2015]]):

$$\boxed{\;\hat{A}^{GAE(\gamma,\lambda)}_t \;=\; \sum_{l=0}^{\infty} (\gamma\lambda)^l\, \delta_{t+l}\;}$$

**Why geometric weights and not, say, uniform:** only the geometric choice collapses like this — and it buys the implementation for free, because the sum obeys a one-line backward recursion over the trajectory:

$$\hat{A}_t = \delta_t + \gamma\lambda\, \hat{A}_{t+1}$$

(compute all the δ's, then one backward pass — this recursion *is* the `compute_gae` function in every PPO codebase).

**Reading the closed form:** the advantage of $a_t$ = the surprise *now*, plus a share of every later surprise, with attribution fading at rate γλ — later surprises are partly the earlier action's doing, but decreasingly so, and beyond ~$\tfrac{1}{1-\gamma\lambda}$ steps the critic has effectively absorbed the rest. The dial, with the endpoints now *derivable* rather than asserted:

- **λ = 0** → only the $l{=}0$ term survives: $\hat{A}_t = \delta_t$ — the one-step [[Actor-Critic]] estimate, maximum trust in the critic
- **λ = 1** → $\sum_l \gamma^l \delta_{t+l}$, which by the Step-1 telescoping (run to episode end, where $V(\text{terminal}) = 0$) collapses to exactly $G_t - V(s_t)$ — the full Monte Carlo advantage; the critic survives only as baseline
- **In between** — the practical zone: the paper's working settings are **γ ∈ [0.99, 0.995], λ ∈ [0.92, 0.98]** — mostly-MC with the distant-tail noise suppressed. E.g. γλ ≈ 0.93 → real surprises count for ~14 steps before the critic takes over

One subtlety worth naming: γ and λ are *different knobs*. γ defines *which problem* you're solving (the horizon of the return itself — the [[Markov Decision Process|discount]]); λ only tunes *how you estimate* advantages for that problem — pure bias–variance, no change of objective.

## Results — what it bought

Paired with trust-region updates for both policy *and* value function: learned **running gaits for 3D bipeds and quadrupeds and getting up off the ground**, with neural policies mapping raw kinematics directly to joint torques — the hardest continuous-control results of their time ([[High-Dimensional Continuous Control Using GAE (2015)|Schulman 2015]]). The honest ledger from the same paper: ~**1–2 weeks of simulated real time** per task — the on-policy sample bill, pre-paid.

## Why this small note carries weight

GAE is the default advantage estimator in essentially every [[PPO]] implementation — which makes it part of [[RLHF]]'s machinery too. And its λ is the *third* appearance of the same dial in this vault (value targets in [[Temporal Difference Learning|TD(λ)]], n-step returns in [[Actor-Critic|A3C]], advantages here): one idea — *geometrically mix bootstrapped and sampled futures* — reused at every level of the stack. When [[GRPO]] later drops the critic entirely, it is choosing λ = 1 and paying the variance bill with parallel rollouts instead.

## Related

- [[Temporal Difference Learning]] — TD(λ), the original dial
- [[Actor-Critic]] — where δ comes from
- [[PPO]] — the consumer
- [[Bellman Equation]] — the advantage, defined

## Sources

- [[High-Dimensional Continuous Control Using GAE (2015)]] — the estimator and the locomotion results

---
Part of the policy-gradient branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
