---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [policy gradients, REINFORCE, log-derivative trick, score function, policy gradient theorem]
---

# Policy Gradient

> **Where this sits.** The second body of the [[Reinforcement Learning|master loop]]: instead of learning values and *deriving* a policy from them (the value-based branch), parameterize the policy $\pi_\theta$ directly and do **gradient ascent on its performance**. IMPROVE becomes a gradient step; the argmax disappears entirely.

## Why go direct? The case against the value detour

Everything so far ([[Q-Learning]], [[Deep Q-Network|DQN]]) learns $Q$ and acts by $\arg\max_a Q(s,a)$ — the policy is a *byproduct*. Five reasons to optimize the policy itself instead:

1. **The argmax doesn't scale to actions.** Enumerating $\max_a$ is fine for 18 Atari buttons, hopeless for continuous torques and unwieldy for structured outputs. A policy network just *emits* the action (or a distribution over it) — action-space size stops being the algorithm's problem
2. **Smoothness.** In value-based control, an ε-sized change in Q-values can *flip* an argmax — the policy jumps discontinuously, and those jumps feed back into the data. In PG, small θ-change ⇒ small policy change by construction; the learning signal moves the behavior *continuously*
3. **Stochastic policies are first-class**, not a bolted-on ε: needed under state aliasing ([[Markov Decision Process|POMDPs]]), and they carry their own exploration ([[Exploration vs Exploitation|Boltzmann-style]])
4. **A real convergence guarantee where the value branch has none:** policy iteration with *arbitrary differentiable* function approximation provably converges to a **locally optimal policy** ([[Policy Gradient Methods with Function Approximation (1999)|Sutton et al. 1999]]) — precisely the setting where value-based methods hit the [[Temporal Difference Learning#The deadly triad|deadly triad]]. The trade: *local* optimum, but guaranteed motion uphill
5. **It's native to LLMs.** A language model *is* $\pi_\theta(a \mid s)$ — a distribution over next tokens given context. Policy gradients update it with what is literally a *weighted* cross-entropy loss (below); no new machinery, no argmax over 100k tokens

The costs, equally honest: **on-policy** (fresh data per update — sample-hungry; the price of dropping the triad's third leg), **high variance** (the entire middle of this note), and **local optima**.

## The objective, and why it seems undifferentiable

Maximize expected return over trajectories the policy generates:

$$J(\theta) = \mathbb{E}_{\tau \sim \pi_\theta}\big[R(\tau)\big], \qquad \tau = (s_0, a_0, r_1, s_1, \dots), \quad R(\tau) = \textstyle\sum_t \gamma^t r_{t+1}$$

The problem: θ enters through the *distribution being sampled from*, not through the reward. You can't backpropagate through the environment — $P(s' \mid s,a)$ is unknown and not differentiable. It looks like a dead end.

## The log-derivative trick, derived

One identity dissolves the problem. For any distribution $p_\theta$ and function $f$:

$$\nabla_\theta\, \mathbb{E}_{x \sim p_\theta}[f(x)] = \mathbb{E}_{x \sim p_\theta}\big[f(x)\, \nabla_\theta \log p_\theta(x)\big]$$

(proof in one line: $\nabla p = p \,\nabla \log p$, insert into the integral). The gradient of an expectation becomes an **expectation of something you can sample** — no derivative of $f$, no derivative of the environment. Now apply it to trajectories. The probability of a trajectory factorizes:

$$\log p_\theta(\tau) = \log p(s_0) + \sum_t \Big[\log \pi_\theta(a_t \mid s_t) + \log P(s_{t+1} \mid s_t, a_t)\Big]$$

and here is the miracle: **the environment terms don't contain θ, so they vanish under $\nabla_\theta$.** The unknown dynamics drop out of the gradient *exactly*, leaving only the policy's own log-probs:

$$\nabla_\theta J = \mathbb{E}_{\tau \sim \pi_\theta}\Big[\sum_t \nabla_\theta \log \pi_\theta(a_t \mid s_t)\; G_t\Big]$$

(with $G_t$ = reward-to-go from $t$ — rewards *before* $t$ multiply out to zero in expectation, since an action can't affect its past). This is **REINFORCE** ([[Simple Statistical Gradient-Following Algorithms - REINFORCE (1992)|Williams 1992]]).

**Read it as an instruction:** *increase the log-probability of every action taken, in proportion to the return that followed.* It is exactly a supervised cross-entropy update on your own sampled actions — **weighted by how well things went**. Good outcome → those actions become more likely; bad outcome → less. That "weighted maximum likelihood" reading is why the formula transfers verbatim to LLMs: it's SFT with per-sample weights.

## The variance problem — and the baseline that fixes it

REINFORCE is unbiased and *painfully* noisy. The core defect: the weight $G_t$ has arbitrary offset. Suppose all returns in a task sit between +100 and +101 — then *every* sampled action gets strongly reinforced, good and bad alike, and the real signal (the ±0.5 difference) is buried under the +100 of common offset. One trajectory's luck swings the whole gradient.

**The fix: subtract a baseline.** For any function $b(s)$ *of state only*:

$$\nabla_\theta J = \mathbb{E}\Big[\sum_t \nabla_\theta \log \pi_\theta(a_t \mid s_t)\,\big(G_t - b(s_t)\big)\Big]$$

is **still unbiased** — the one-line proof: $\mathbb{E}_{a\sim\pi}[\nabla \log \pi(a \mid s)\, b(s)] = b(s)\, \nabla_\theta \underbrace{\textstyle\sum_a \pi(a \mid s)}_{=\,1} = 0$. Probabilities sum to 1, so a state-dependent constant pushes all actions up and down equally — net zero effect on the gradient, potentially huge effect on its variance ([[Simple Statistical Gradient-Following Algorithms - REINFORCE (1992)|Williams 1992]]).

The near-optimal choice is $b(s) = V^\pi(s)$ — "how well was I *expecting* to do from here" — which turns the weight into the **advantage**:

$$G_t - V^\pi(s_t) \approx A^\pi(s_t, a_t) = \text{“how much better than my own average was this action?”}$$

Now the instruction reads: reinforce actions in proportion to how much they **beat expectations** — zero-mean by construction ([[Bellman Equation|the advantage]], now doing the job it was defined for). The remaining question — *how to estimate $V^\pi$ and the advantage from samples* — is answered by learning a critic with [[Temporal Difference Learning|TD]]: the subject of [[Actor-Critic]] and [[Generalized Advantage Estimation]].

## The policy gradient theorem — why sampling this is legitimate at all

One subtlety the derivation swept past: changing θ changes *which states you visit*, and you might expect a term for the gradient of the state distribution — a term you could never estimate (it depends on the unknown dynamics). **The policy gradient theorem** proves the expectation above is the true gradient *without any such term*: state-visitation shift is already accounted for when sampling states from the policy's own trajectories ([[Policy Gradient Methods with Function Approximation (1999)|Sutton et al. 1999]]). Also there: **compatible function approximation** — conditions under which a *learned* critic keeps the gradient unbiased — and the local-optimum convergence proof. This theorem is why the entire branch stands on solid ground where the value branch needed engineering patches.

## Where it goes from here

The skeleton is fixed; everything after is variance and step-size engineering: a TD-learned baseline ([[Actor-Critic]]), a tunable bias–variance advantage dial ([[Generalized Advantage Estimation]]), and a safe way to take *more than one* gradient step per batch ([[PPO]]) — the path that leads directly to [[RLHF]].

## Related

- [[Reinforcement Learning]] — the loop this branch implements with gradients
- [[Bellman Equation]] — the advantage function, now the star
- [[Actor-Critic]] · [[Generalized Advantage Estimation]] · [[PPO]] — the refinement chain
- [[Gradient Descent]] — the optimizer underneath

## Sources

- [[Simple Statistical Gradient-Following Algorithms - REINFORCE (1992)]] — the founding algorithm + baselines
- [[Policy Gradient Methods with Function Approximation (1999)]] — the theorem and convergence
- [[Reinforcement Learning - An Introduction (1998)]] — framing

---
Part of the policy-gradient branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
