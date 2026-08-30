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

## The key substitution: from G − V to δ

Start from where [[Policy Gradient]] left off. The refined gradient weights each action's $\nabla \log \pi$ by the Monte Carlo advantage estimate:

$$\underbrace{G_t - V(s_t)}_{\text{"how much the actual future beat the prediction"}}$$

— unbiased, but it carries the randomness of *every future reward*, and you must wait for the episode to end. Now do to this weight exactly what [[Temporal Difference Learning|TD]] did to value targets. The return telescopes, $G_t = r_{t+1} + \gamma\, G_{t+1}$, so:

$$G_t - V(s_t) \;=\; r_{t+1} + \gamma\, \underbrace{G_{t+1}}_{\text{the entire random future}} - \;V(s_t)$$

**The actor-critic move: replace the random tail $G_{t+1}$ with the critic's estimate $V_\phi(s_{t+1})$:**

$$G_t - V(s_t) \;\;\leadsto\;\; r_{t+1} + \gamma\, V_\phi(s_{t+1}) - V_\phi(s_t) \;=\; \delta_t$$

So yes — **δ *is* the advantage estimate, with all-but-one of $G$'s terms collapsed into a bootstrap**. What the swap buys and costs, precisely:

- **Variance collapses:** the weight now contains *one* random reward plus two deterministic numbers, instead of a sum of hundreds of random rewards. And it's available *immediately* — no waiting for episode end
- **Bias enters — exactly where you'd expect:** during training $V_\phi \neq V^\pi$, so δ systematically mis-scores actions wherever the critic is wrong. The clean identity $\mathbb{E}[\delta_t \mid s_t, a_t] = A^\pi(s_t, a_t)$ (via the [[Bellman Equation|Bellman relation]] $\mathbb{E}[r + \gamma V^\pi(s')] = Q^\pi(s,a)$, minus $V^\pi(s)$) holds **only for the *true* $V^\pi$** — with the learned $V_\phi$, δ is a *biased* advantage estimate whose bias shrinks as the critic improves. Same self-correcting structure as TD itself, and also actor-critic's characteristic instability: a wrong critic misdirects the actor, which then collects data shaped by the misdirection
- **It's a dial, not a binary:** δ is the fully-bootstrapped corner; $G_t - V(s_t)$ is the no-bootstrap corner; n-step versions ($r_{t+1} + \gamma r_{t+2} + \cdots + \gamma^n V_\phi(s_{t+n}) - V_\phi(s_t)$) sit between; and [[Generalized Advantage Estimation]] is the geometric mix of all of them — the whole MC↔TD trade, now living inside the policy gradient's *weight*

## The mechanics: one transition, two updates

Experience $(s_t, a_t, r_{t+1}, s_{t+1})$, compute $\delta_t$, and update both networks *from the same number*:

$$\text{critic:}\quad \phi \leftarrow \phi + \alpha_c\, \delta_t\, \nabla_\phi V_\phi(s_t) \qquad\qquad \text{actor:}\quad \theta \leftarrow \theta + \alpha_a\, \delta_t\, \nabla_\theta \log \pi_\theta(a_t \mid s_t)$$

Two different readings of one δ: the **critic's update is just TD** ($\delta \nabla V$ is the semi-gradient step on the squared error toward the target $r + \gamma V_\phi(s')$); the **actor's update is just the policy gradient** with δ standing in for the advantage — *raise the log-prob of the action by how much it beat the critic's expectation*. The elegant economy: **the surprise that trains the critic is the judgment that trains the actor.**

Tiny numeric run: $V_\phi(s_t) = 2$, $V_\phi(s_{t+1}) = 6$, $r = 1$, γ = 0.5 → $\delta = 1 + 3 - 2 = +2$: the action's log-probability gets pushed up (weight +2) *and* the prediction at $s_t$ gets raised — both networks conclude "that went better than expected," each in its own currency. A Monte Carlo version of the same update couldn't fire until the episode ended. (δ > 0 → reinforce and raise; δ < 0 → suppress and lower — also the standard computational reading of dopamine signaling, the TD error made flesh.)

## The deep-RL landmark: A3C, and parallelism as the on-policy replay

Naive deep actor-critic hits the same correlated-data problem as naive [[Deep Q-Network|deep Q-learning]]: consecutive transitions are near-duplicates, and SGD wants independent samples. But the DQN cure is illegal here. **Why a replay buffer breaks a policy gradient:** the estimator $\mathbb{E}_{a \sim \pi_\theta}[\delta\, \nabla \log \pi_\theta(a \mid s)]$ is an average over actions drawn *from the current policy* — that's what makes it the gradient of the current policy's performance. A transition replayed from a month-old buffer has its action drawn from $\pi_{old}$: plugging it in weights the current gradient by the *wrong sampling distribution*, and the estimator silently stops pointing uphill. (Correcting the mismatch with probability ratios is possible — that road leads to [[PPO]].)

**A3C's** answer keeps everything on-policy ([[Asynchronous Methods for Deep RL - A3C (2016)|Mnih 2016]]): run **16 parallel actor-learners**, each on its own environment copy, asynchronously updating shared weights. Look at what a combined gradient batch contains at any instant: worker 3 is mid-boss-fight, worker 9 just respawned, worker 14 is in a level the others haven't reached — a *cross-section* of many episodes and situations rather than a filmstrip of one. Decorrelation **by parallelism instead of by memory**, with every sample fresh from the current policy.

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
