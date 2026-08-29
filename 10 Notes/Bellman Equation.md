---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [Bellman equations, value function, value iteration, policy iteration, dynamic programming]
---

# Bellman Equation

## Why value functions exist

The [[Markov Decision Process|MDP]] objective is the expected return $\mathbb{E}[G_t]$ — a sum over the *entire future*. To act well you constantly need to compare futures: "if I land in state $s$, how good is my life from there?" The **value function** compresses that infinite future into one number per state:

$$V^\pi(s) = \mathbb{E}_\pi[G_t \mid s_t = s] \qquad \text{"expected total reward from } s \text{, if I follow policy } \pi \text{ afterwards"}$$

Computed naively, this is an expectation over infinitely many infinitely-long trajectories — hopeless. The Bellman equation is the observation that makes it computable at all.

## The recursion: one step of lookahead replaces the infinite sum

The return telescopes: $G_t = r_{t+1} + \gamma\, G_{t+1}$ — total future reward = the next reward + the discounted total after that. Take expectations of both sides and the value of *now* becomes reward plus the value of *next*:

$$V^\pi(s) = \underbrace{\sum_a \pi(a \mid s)}_{\text{actions } \pi \text{ might take}} \underbrace{\sum_{s'} P(s' \mid s,a)}_{\text{where the world lands}} \big[\underbrace{r(s,a)}_{\text{paid now}} + \gamma\, \underbrace{V^\pi(s')}_{\text{everything after}}\big]$$

This is the **Bellman expectation equation**. Nothing was approximated — the infinite horizon is hiding inside $V^\pi(s')$, and the equation just says the values at neighboring states must be *consistent* with each other. One equation per state, $|\mathcal{S}|$ unknowns: a solvable system instead of an infinite simulation. That single move — replace "sum over futures" with "local consistency between neighbors" — is dynamic programming, and it's the trick every value-based RL method inherits.

## The four equations

Two axes: value of *states* ($V$) vs *state–action pairs* ($Q$), and value of *a given policy* vs *the best possible* ($^*$):

| Object | Meaning | Bellman form |
|---|---|---|
| $V^\pi(s)$ | how good is $s$ under $\pi$ | $V^\pi(s) = \sum_a \pi(a \mid s) \sum_{s'} P(s' \mid s,a)\,[r + \gamma V^\pi(s')]$ |
| $Q^\pi(s,a)$ | how good is doing $a$ in $s$, then following $\pi$ | $Q^\pi = r(s,a) + \gamma \sum_{s'} P \sum_{a'} \pi(a' \mid s')\, Q^\pi(s',a')$ |
| $V^*(s)$ | how good is $s$ if you play perfectly | $V^*(s) = \max_a \sum_{s'} P(s' \mid s,a)\,[r + \gamma V^*(s')]$ |
| $Q^*(s,a)$ | how good is $a$ in $s$, then perfect play | $Q^* = r(s,a) + \gamma \sum_{s'} P\, \max_{a'} Q^*(s',a')$ |

Two structural facts to read off:

- The **expectation** equations (top) are **linear** in the values → evaluating a fixed policy is "just" solving a linear system. The **optimality** equations (bottom) contain a **max** → nonlinear, and the real object of RL
- **Why algorithms learn $Q$ rather than $V$:** given $Q^*$, acting optimally is a lookup — $\pi^*(s) = \arg\max_a Q^*(s,a)$. Given only $V^*$, choosing an action requires imagining outcomes ("which $a$ leads to high-value states?"), i.e. it requires knowing $P$. $Q$ has the one-step lookahead *baked in* — that's the entire reason model-free control works

One derived quantity worth naming now: the **advantage** $A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)$ — "how much better is action $a$ than what $\pi$ would do on average here." It's zero-mean under $\pi$, which makes it the natural low-variance learning signal — the currency of all [[Policy Gradient|policy-gradient]] methods.

## Why a solution exists at all: the contraction

Define the **Bellman operator** $T$: take any guess table $V$ (even garbage), and back it up one step everywhere — $(TV)(s) = \max_a \sum_{s'} P(s' \mid s,a)[r + \gamma V(s')]$. The key property ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]):

$$\|TV - TU\|_\infty \le \gamma\,\|V - U\|_\infty$$

**In words: applying $T$ to two different guesses pulls them at least a factor γ closer** (in the worst-case entry). Why: the two guesses only enter through the future term, which is weighted by γ. Consequences, all free from Banach's fixed-point theorem:

- **Existence + uniqueness:** a contraction has exactly one fixed point — and a fixed point of $T$ is precisely a table satisfying the optimality equation, i.e. $V^*$. So $V^*$ exists, is unique, and iterating $T$ from *any* start converges to it
- **Speed:** error shrinks geometrically, $\|V_k - V^*\|_\infty \le \gamma^k \|V_0 - V^*\|_\infty$; precision $\epsilon$ takes $k \approx \tfrac{\ln(1/\epsilon)}{1-\gamma}$ sweeps — the $\tfrac{1}{1-\gamma}$ horizon tax from the [[Markov Decision Process|MDP note]] again
- **Robustness:** acting greedily on an *approximate* $V$ with error $\epsilon$ costs at most $\tfrac{2\gamma\epsilon}{1-\gamma}$ in policy quality ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]]) — small value errors stay small, but get amplified by the horizon

## The two classical solvers

- **Value iteration (VI):** literally iterate $V_{k+1} = TV_k$ until values stop changing, then act greedily. Simple, global, but converges at the linear rate γ — slow when γ ≈ 1
- **Policy iteration (PI):** alternate two steps — **evaluate** the current policy exactly (solve the linear expectation equation for $V^{\pi_k}$), then **improve** it (make the policy greedy w.r.t. $V^{\pi_k}$: in each state, switch to the action that looks best under the just-computed values). Each round produces a policy at least as good in every state; with finitely many policies, it terminates exactly ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])
- **Result — why PI converges in so few rounds:** PI is exactly the **(semismooth) Newton method** applied to the Bellman equation, so it inherits **local quadratic convergence**, while VI is plain fixed-point iteration ([[Dynamic Programming as Semismooth Newton (2022)|Gargiani 2022]]). PI : VI :: Newton : gradient descent — the empirical "PI needs ~5 iterations on huge MDPs" finally has a textbook explanation
- Also solvable as a **linear program** (min $\sum_s V(s)$ s.t. $V \ge TV$); its dual optimizes over state–action *occupancy measures* — the formulation offline-RL and constrained-RL theory builds on ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])

Both solvers assume $P$ and $R$ are **known** and $|\mathcal{S}|$ is **enumerable**. RL is what remains when neither holds.

## From equations to algorithms: sample the backup

No model → you can't compute the expectation $\sum_{s'} P(s' \mid s,a)[\cdot]$ — but you can *experience* one sample of it: act, observe $(s, a, r, s')$, and nudge your estimate toward the sampled backup. Each classical solver has a sampled shadow:

| Bellman equation being solved | sampled, incremental version |
|---|---|
| expectation eq. for $V^\pi$ | [[Temporal Difference Learning\|TD(0)]] |
| optimality eq. for $Q^*$ | [[Q-Learning]] (sampled value iteration) |
| expectation eq. for $Q^\pi$, with $\pi$ improving | SARSA, [[Actor-Critic]] critics |

**Conclusion:** everything above is guaranteed by a contraction that lives in **sup-norm over exact tables**. Swap tables for neural networks and expectations for off-policy samples, and the contraction argument dissolves — convergence becomes conditional, and sometimes fails spectacularly. That boundary (the *deadly triad*) is drawn precisely in [[Temporal Difference Learning]].

## Related

- [[Markov Decision Process]] — the formalism these equations live on
- [[Temporal Difference Learning]] — sample-based Bellman solving
- [[Monte Carlo Tree Search]] — Bellman backups on a tree, computed on demand

## Sources

- [[Markov Decision Processes - Puterman (1994)]] — existence, uniqueness, VI/PI/LP
- [[Dynamic Programming and Optimal Control - Bertsekas (1995)]] — error amplification bounds
- [[Dynamic Programming as Semismooth Newton (2022)]] — PI = Newton, quadratic rate
- [[Reinforcement Learning - An Introduction (1998)]] — notation and the algorithmic framing

---
Part of the RL foundations cluster — map: [[RL MOC]].
