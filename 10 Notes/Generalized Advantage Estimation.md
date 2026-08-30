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

## The estimator: exponentially mix all of them

**GAE(γ, λ)** averages every n-step estimator with geometrically decaying weights — which telescopes into a beautifully simple form: *a discounted sum of the TD errors themselves* ([[High-Dimensional Continuous Control Using GAE (2015)|Schulman 2015]]):

$$\hat{A}^{GAE(\gamma,\lambda)}_t \;=\; \sum_{l=0}^{\infty} (\gamma\lambda)^l\, \delta_{t+l}$$

Read it as: the advantage of action $a_t$ = the surprise *now*, plus a discounted share of every surprise that follows (later surprises partly attributable to the earlier action, with attribution fading at rate γλ). The dial:

- **λ = 0** → $\hat{A}_t = \delta_t$ — the one-step [[Actor-Critic]] estimate; maximum trust in the critic
- **λ = 1** → the full Monte Carlo advantage $G_t - V(s_t)$; the critic serves only as baseline
- **In between** — the practical zone: the paper's working settings are **γ ∈ [0.99, 0.995], λ ∈ [0.92, 0.98]** — mostly-MC with the tail noise suppressed

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
