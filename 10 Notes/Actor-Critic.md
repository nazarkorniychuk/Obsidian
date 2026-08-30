---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [actor-critic, A2C, A3C, advantage actor-critic]
---

# Actor-Critic

> **Where this sits.** The marriage of the two branches: a **policy network (the actor)** improved by [[Policy Gradient|policy gradients]], plus a **value network (the critic)** learned by [[Temporal Difference Learning|TD]]. It is the [[Reinforcement Learning|master loop]] built directly into an architecture — the critic *is* EVALUATE, the actor update *is* IMPROVE, running simultaneously.

## The motivation: REINFORCE's two waits

Pure REINFORCE has the [[Policy Gradient|right gradient]] but pays twice for using raw Monte Carlo returns $G_t$:

1. **It waits for episode end** — no learning mid-episode, nothing at all in continuing tasks
2. **It carries full MC variance** — $G_t$ sums the noise of every reward until termination; one lucky trajectory swings the entire gradient

You've seen this exact disease and its cure before: it's the MC-vs-TD trade from [[Temporal Difference Learning]], now happening inside the *weight* of the policy gradient instead of inside a value table. The cure is the same — **bootstrap**. Learn a value estimate $V_\phi$ (the critic) and use it twice:

- as the **baseline** (variance reduction, unbiased — the [[Policy Gradient|baseline theorem]])
- inside the **target** (bootstrapping — updates every step, at the price of the critic's bias)

## The mechanics: one transition, two updates

Experience $(s_t, a_t, r_{t+1}, s_{t+1})$, compute the familiar TD error:

$$\delta_t = r_{t+1} + \gamma\, V_\phi(s_{t+1}) - V_\phi(s_t)$$

Then update both networks *from the same number*:

$$\text{critic:}\quad \phi \leftarrow \phi + \alpha_c\, \delta_t\, \nabla_\phi V_\phi(s_t) \qquad\qquad \text{actor:}\quad \theta \leftarrow \theta + \alpha_a\, \delta_t\, \nabla_\theta \log \pi_\theta(a_t \mid s_t)$$

Why the *same* δ serves both: for the critic it's the usual [[Temporal Difference Learning|TD]] learning signal. For the actor, δ is a **one-sample estimate of the advantage** — $\mathbb{E}[\delta_t \mid s_t, a_t] = A^\pi(s_t, a_t)$, since $\mathbb{E}[r + \gamma V(s')] = Q(s,a)$ and subtracting $V(s)$ gives the advantage. So the actor's update reads exactly like the refined policy gradient: *raise the log-prob of the action by how much it beat the critic's expectation.* The elegant economy of the architecture: **the surprise that trains the critic is the judgment that trains the actor.**

Interpretation worth keeping: δ > 0 — "that went better than I predicted" → action reinforced *and* prediction raised; δ < 0 — both lowered. (This is also the standard computational reading of dopamine signaling — the TD error made flesh.)

## The deep-RL landmark: A3C, and parallelism as the on-policy replay

Naive deep actor-critic hits the same correlated-data problem as naive [[Deep Q-Network|deep Q-learning]] — but it *can't use a replay buffer*, because policy gradients are on-policy (old policies' actions carry the wrong log-probs). **A3C's** answer ([[Asynchronous Methods for Deep RL - A3C (2016)|Mnih 2016]]): run **16 parallel actor-learners**, each on its own environment copy, asynchronously updating shared weights. At any instant the workers occupy different episodes and situations — the combined gradient stream is decorrelated **by parallelism instead of by memory**, and everything stays perfectly on-policy.

- **Results:** surpassed the then-SOTA on Atari in **half the training time on a multi-core CPU** (no GPU); same framework handled continuous motor control and 3D mazes from pixels
- The full recipe — parallel rollouts, **n-step returns** (the [[Temporal Difference Learning|bias–variance dial]] set a few notches from pure TD), an **entropy bonus** keeping the policy from collapsing ([[Exploration vs Exploitation|strategy-3 exploration]], built into the loss) — became *the* on-policy template
- The asynchrony itself turned out inessential: the synchronous variant **A2C** (collect from all workers, one big batch, one update) works as well and uses hardware better. The substance was parallel *data collection*, not lock-free updates

Later large-scale tuning studies filled in what else matters in this template: near-uniform policy initialization (tiny last-layer weights — one of the largest single effects measured), observation and advantage normalization, Adam at ~3e-4 ([[What Matters in On-Policy RL (2020)|Andrychowicz 2020]]).

## Where the branch stands now

Actor-critic is the *chassis*; what remains are two refinements bolted onto it: a properly tunable advantage estimator ([[Generalized Advantage Estimation]] — the λ-dial applied to δ's), and a mechanism for taking several safe gradient steps per batch of rollouts ([[PPO]]). In LLM terms the chassis maps directly: the actor is the language model, the critic is a **value head** on the same backbone, δ-style advantages weight per-token log-probs — [[RLHF]] is this note at scale.

## Related

- [[Policy Gradient]] — the actor's update, derived
- [[Temporal Difference Learning]] — the critic's update, derived
- [[Generalized Advantage Estimation]] · [[PPO]] — the two refinements
- [[Deep Q-Network]] — the value-based sibling; replay vs parallelism as decorrelators

## Sources

- [[Asynchronous Methods for Deep RL - A3C (2016)]] — the deep actor-critic landmark
- [[Policy Gradient Methods with Function Approximation (1999)]] — compatible critics, convergence
- [[What Matters in On-Policy RL (2020)]] — what actually moves the needle
- [[Reinforcement Learning - An Introduction (1998)]] — the actor-critic idea's origin

---
Part of the policy-gradient branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
