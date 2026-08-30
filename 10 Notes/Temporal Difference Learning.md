---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [TD learning, TD, TD(0), TD(lambda), eligibility traces, bootstrapping]
---

# Temporal Difference Learning

> **The foundations pipeline — who solves what.** [[Markov Decision Process]] poses the problem → [[Bellman Equation]] solves it *on paper* when the rulebook $(P,R)$ is known (planning) → **this note solves it *from experience* when the rulebook is missing — *learning*: the same Bellman backups, rebuilt from one sampled transition at a time** → [[Exploration vs Exploitation]] supplies the experience — because, as shown below, TD can only update what the policy actually visits.

## The problem: Bellman machinery without the model

Value iteration and policy iteration ([[Bellman Equation]]) compute values by taking expectations over $P(s' \mid s,a)$ — they need the model. An RL agent doesn't have it. All it has is *experience*: a stream of transitions $(s, a, r, s')$ from acting in the world. TD learning is the answer to: **how do you learn a value function from experience alone?** It is the single idea underneath [[Q-Learning]], SARSA, the critics in [[Actor-Critic]] methods, and [[Generalized Advantage Estimation|GAE]].

The core intuition, before any formula. You leave the office predicting the commute home takes **30 min**. On the highway you hit stopped traffic; you now predict **50 min** total. Two ways to learn from this:

- **Wait until you're home**, measure the true total, and update your "leaving the office" estimate toward it — learning from *outcomes*
- **Update right now**: traffic already proved the 30 was too low. Shift the office estimate toward 50 immediately — learning from *a change in your own prediction*

The second is TD: **successive predictions about the same future must agree, and disagreement is a training signal** — available at every step, before the outcome exists ([[Learning to Predict by the Methods of Temporal Differences (1988)|Sutton 1988]]).

## The update rule, derived

Take the Bellman expectation equation, $V^\pi(s) = \mathbb{E}[\,r + \gamma V^\pi(s')\,]$. You can't compute the expectation (no $P$) — but each experienced transition is **one sample** of exactly that expectation. So nudge the estimate toward the sample:

$$V(s_t) \leftarrow V(s_t) + \alpha \, \big[\underbrace{r_{t+1} + \gamma V(s_{t+1})}_{\text{target: the sampled backup}} - \underbrace{V(s_t)}_{\text{current estimate}}\big]$$

The bracket is the **TD error**:

$$\delta_t = r_{t+1} + \gamma V(s_{t+1}) - V(s_t)$$

— the *surprise*: what one step of reality said, minus what you predicted. $\delta > 0$: things went better than expected, raise $V(s_t)$; $\delta < 0$: lower it. $\alpha$ is a step size, exactly like a learning rate in [[Gradient Descent]].

Tiny numeric run: table says $V(A) = 2$, $V(B) = 6$; γ = 0.5, α = 0.1. You experience $A \xrightarrow{r=1} B$. Target $= 1 + 0.5 \cdot 6 = 4$; error $\delta = 4 - 2 = +2$; update $V(A) \leftarrow 2 + 0.1 \cdot 2 = 2.2$.

Notice what sits *inside* the target: your own estimate $V(s_{t+1})$. Using your current guess as part of the truth you train on is called **bootstrapping** — it's what makes updates available immediately, and it's also the source of every complication in this note.

## Same backup as value iteration — three substitutions

TD is [[Bellman Equation|value iteration]] forced through the keyhole of experience. Side by side, it's the identical Bellman backup with three substitutions, each forced by not having the model:

| | value iteration (planning) | TD / Q-learning (learning) |
|---|---|---|
| the expectation over $P(s' \mid s,a)$ | computed exactly from the table | replaced by **one sample** — the $(r, s')$ that actually happened |
| which entries update | **all states**, every sweep | **only the state you actually visited** |
| step size | full replacement — no noise to fear | small α, averaging out sampling noise |

The middle row is the one with teeth. VI updates every entry by fiat; TD updates only what behavior touches. Consequence: if the policy never takes action $B$ in state $s$, then $Q(s,B)$ is **never updated** — it just sits at its initialization. The greedy policy is computed from the table *including that garbage entry*, so it keeps avoiding $B$, so $B$ keeps never updating: **the loop can permanently lock in on its own ignorance** — even in a fully deterministic world. This is why every convergence theorem below carries the precondition "all $(s,a)$ visited infinitely often": TD doesn't *provide* exploration, it **demands it as an input** — and [[Exploration vs Exploitation]] is the note about supplying it.

## The alternative: Monte Carlo — and the trade between them

The learn-from-outcomes option also exists: wait for the episode to end, compute the actual return $G_t$, and update $V(s_t) \leftarrow V(s_t) + \alpha[G_t - V(s_t)]$. That's **Monte Carlo (MC)**. The comparison defines the whole design space:

| | target | biased? | variance | update when? | needs |
|---|---|---|---|---|---|
| MC | actual return $G_t$ | no | **high** — sums every future reward's randomness | episode end | episodes that end |
| TD(0) | $r + \gamma V(s')$ | **yes**, while $V(s')$ is wrong | low — one reward + one estimate | every step | nothing extra |

- **Why MC's variance is high:** $G_t$ is a sum of hundreds of random rewards through random states; a single lucky trajectory swings the target wildly. TD's target contains one random reward and one (deterministic, if wrong) estimate
- **Why TD is biased:** during learning $V(s')$ is off, so you chase a wrong target. Bias decays as estimates improve — the same self-correcting structure as value iteration
- **Results** ([[Learning to Predict by the Methods of Temporal Differences (1988)|Sutton 1988]]): on the random-walk prediction task, with *finite data*, TD is **more accurate on future data than the supervised/MC fit of the same data** — TD exploits the Markov structure (paths share states, information propagates between them) that MC ignores. Also less memory and less peak computation: no stored episodes, updates are incremental
- The DP–MC–TD triangle in one line: DP bootstraps with a model, MC samples without bootstrapping, **TD samples *and* bootstraps** ([[Reinforcement Learning - An Introduction (1998)|Sutton & Barto]])

## TD(λ): a dial between the two extremes

Why choose one step of reality or all of it? The **n-step return** takes $n$ real rewards before trusting the estimate: $G_t^{(n)} = r_{t+1} + \gamma r_{t+2} + \cdots + \gamma^{n-1} r_{t+n} + \gamma^n V(s_{t+n})$. The **λ-return** mixes all of them with geometric weights:

$$G_t^\lambda = (1-\lambda)\sum_{n\ge1} \lambda^{\,n-1}\, G_t^{(n)} \qquad \lambda = 0 \to \text{TD(0)}, \quad \lambda = 1 \to \text{MC}$$

Waiting for all n-step returns would kill the online property, so the implementation runs *backward*: every state keeps an **eligibility trace** $e(s)$ — a decaying "recently visited, still responsible" marker, bumped on visit and decayed by $\gamma\lambda$ per step. When a TD error $\delta_t$ occurs, **all** eligible states update in proportion to their trace: credit flows backward through the trajectory at $O(\text{params})$ per step. **Results:** TD(λ) converges for all λ — in expectation ([[The Convergence of TD(lambda) for General lambda (1992)|Dayan 1992]]), then with probability 1 (Dayan & Sejnowski 1994). In practice intermediate λ ≈ 0.9–0.97 beats both endpoints — the identical dial reappears as GAE's λ in [[PPO]].

## Function approximation: where the guarantees end

Real state spaces don't fit in tables ([[Markov Decision Process|curse of dimensionality]]), so $V_\theta(s)$ becomes a parametric function and the update becomes *semi-gradient*: $\theta \leftarrow \theta + \alpha\,\delta_t\, \nabla_\theta V_\theta(s_t)$ — "semi" because the target also depends on θ but is treated as a constant. [[An Analysis of Temporal-Difference Learning with Function Approximation (1997)|Tsitsiklis & Van Roy 1997]] drew the exact boundary of what survives:

- **Positive:** linear $V_\theta$, trained **on-policy** (states weighted as the policy actually visits them), converges w.p.1, with error at most $\tfrac{1-\lambda\gamma}{1-\gamma} \times$ the best error any linear function could achieve — larger λ provably tightens the bound
- **Negative:** weight updates by any *other* state distribution and TD can **diverge**; nonlinear approximators can diverge even on-policy
- Canonical demo: **Baird's counterexample** — 7 states, linearly independent features, exact solution representable, and off-policy Q-learning's values run off to infinity anyway ([[Off-Policy TD with Function Approximation - Precup (2001)|Precup 2001]])
- **The deadly triad** ([[Reinforcement Learning - An Introduction (1998)|Sutton & Barto]]'s name): (1) function approximation, (2) bootstrapping, (3) off-policy data. Any two are safe; all three can diverge. Why they conspire: approximation makes each update *leak* to other states, bootstrapping makes the target *move* when θ moves, off-policy weighting means errors grow at states the data rarely corrects
- **Conclusion:** [[Deep Q-Network|DQN]]'s target network (freeze the bootstrap target → less leg 2) and replay-near-on-policy (→ less leg 3) are engineering patches for exactly this
- Modern quantitative rates: TD ≈ online gradient descent, $O(1/\sqrt{T})$ with averaging, under Markovian sampling ([[A Finite Time Analysis of Temporal Difference Learning (2018)|Bhandari 2018]]) — notable because the TD update is *not* the gradient of any fixed loss

## TD control: SARSA and Q-learning

To *act*, learn $Q$ instead of $V$ (argmax needs no model — the [[Bellman Equation|Q-vs-V argument]]). One experienced step $(s, a, r, s', a')$ gives two different sampled backups:

- **SARSA** (on-policy): $\delta = r + \gamma\, Q(s', a') - Q(s,a)$ with $a'$ = the action *actually taken next*. Learns the value of the policy being followed, exploration noise included
- **[[Q-Learning]]** (off-policy): $\delta = r + \gamma \max_{a'} Q(s', a') - Q(s,a)$. Learns the value of the *greedy* policy while behaving any exploratory way whatsoever — sampled value iteration. **Result:** converges to $Q^*$ w.p.1 if every $(s,a)$ is visited infinitely often and steps satisfy Robbins–Monro: $\sum \alpha = \infty$ (big enough to get anywhere) but $\sum \alpha^2 < \infty$ (shrinking so sampling noise averages out) ([[Q-learning - Watkins & Dayan (1992)|Watkins & Dayan 1992]])

## The result that made TD famous

**TD-Gammon**: TD(λ) + a neural network + backgammon **self-play**, starting from zero knowledge. **Results:** stronger than commercial programs, and — the striking part — **better than the same network trained supervised on a massive human-expert dataset**; later versions reached world-champion level and changed human opening theory ([[Practical Issues in Temporal Difference Learning - TD-Gammon (1992)|Tesauro 1992]]). A 25-years-early demonstration of both halves of the modern story: nonlinear TD works far better in practice than the theory guarantees (data stays on-policy under self-play), and self-play beats imitating experts — the direct ancestor of AlphaGo/[[Monte Carlo Tree Search|AlphaZero]].

## Where it lives in LLM training

The critic in [[PPO]]-style [[RLHF]] is a value function trained by TD-style bootstrapping, and [[Generalized Advantage Estimation|GAE]] is literally the λ-return applied to advantages. When [[GRPO]] drops the critic, it is choosing the MC corner of the MC↔TD trade — unbiased, higher variance, no bootstrap pathologies — affordable because LLM episodes are short and parallel.

## Related

- [[Bellman Equation]] — TD = sampled Bellman backup
- [[Q-Learning]] · [[Deep Q-Network]] — TD control and its deep-RL patches
- [[Actor-Critic]] · [[Generalized Advantage Estimation]] — TD errors as policy-gradient signal
- [[Gradient Descent]] — TD is *not* SGD on a fixed loss; that's the whole subtlety

## Sources

- [[Learning to Predict by the Methods of Temporal Differences (1988)]] — the founding paper
- [[The Convergence of TD(lambda) for General lambda (1992)]] — tabular convergence
- [[An Analysis of Temporal-Difference Learning with Function Approximation (1997)]] — the convergence boundary
- [[Off-Policy TD with Function Approximation - Precup (2001)]] — Baird divergence record
- [[A Finite Time Analysis of Temporal Difference Learning (2018)]] — finite-time rates
- [[Q-learning - Watkins & Dayan (1992)]] — off-policy TD control
- [[Practical Issues in Temporal Difference Learning - TD-Gammon (1992)]] — the landmark application
- [[Reinforcement Learning - An Introduction (1998)]] — DP/MC/TD unification, deadly triad

---
Part of the RL foundations cluster — map: [[RL MOC]].
