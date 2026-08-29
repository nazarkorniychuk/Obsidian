---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [MDP, MDPs, Markov decision processes]
---

# Markov Decision Process

The problem statement of reinforcement learning: an agent picks actions, the environment moves stochastically and pays reward, the agent maximizes cumulative reward. Everything in the [[RL MOC|RL cluster]] is an algorithm for some MDP.

## ⚡ The formalism

| Object | Definition | Role |
|---|---|---|
| $\mathcal{S}, \mathcal{A}$ | state and action sets | what exists |
| $P(s'\mid s,a)$ | transition kernel | environment dynamics |
| $R(s,a)$ | reward function | the objective, one step at a time |
| $\gamma \in [0,1)$ | discount factor | horizon control (below) |
| $\pi(a\mid s)$ | policy | the thing being learned |
| $G_t = \sum_{k=0}^{\infty}\gamma^k r_{t+k+1}$ | return | the quantity maximized; $\|G\| \le r_{\max}/(1-\gamma)$ |

Goal: $\max_\pi \mathbb{E}_\pi[G_t]$. Values of states/actions under this objective are the subject of [[Bellman Equation]].

## The Markov property is the load-bearing assumption

$P(s_{t+1} \mid s_t, a_t) = P(s_{t+1} \mid s_0, a_0, \dots, s_t, a_t)$ — the state is a **sufficient statistic of history**: for predicting the future, knowing more of the past adds nothing. Markov-ness is a property of your *state design*, not of the world: a ball's position alone is not Markov (rising vs falling), (position, velocity) is — history gets absorbed into the state.

**Result** ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]): for finite discounted MDPs an optimal policy always exists that is

- **stationary** — same rule at every timestep, $\pi(a\mid s)$ doesn't depend on $t$. (Holds because the infinite discounted future looks identical from $s$ at any time; *fails* for finite horizons, where time-left changes the best action)
- **deterministic** — one fixed action per state, no randomization. (Randomizing averages Q-values; an average never beats $\arg\max_a Q^*(s,a)$)

So the search space collapses from all history-dependent randomized rules to the **finite set of $|\mathcal{A}|^{|\mathcal{S}|}$ lookup tables** — why policy iteration can terminate finitely at all.

**POMDP** (partially observable MDP): you don't see the state $s_t$, only an observation $o_t \sim O(\cdot\mid s_t)$ — a camera frame, not the world. Different states can emit the same observation (*aliasing*), so the future now genuinely depends on history. The fix is the **belief state** $b_t(s) = P(s_t{=}s \mid o_{1:t}, a_{1:t})$ — a probability distribution over states, updated by Bayes each step. That makes it an MDP again, but over a *continuous* space of distributions: qualitatively harder (exact planning is PSPACE-hard vs polynomial for MDPs). Deep agents approximate the belief cheaply: **frame-stacking** (4 Atari frames ⇒ velocity recoverable) or **recurrence** (hidden state = learned compressed history)

## Discounting: γ is a horizon knob, not a detail

![[mdp-discount-horizon.png|460]]

- $\gamma < 1$ makes infinite-horizon returns finite ($\le r_{\max}/(1-\gamma)$) and makes the Bellman operator a **γ-contraction** — it is simultaneously the modeling choice and the convergence mechanism ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])
- **Effective horizon** $\approx 1/(1-\gamma)$: the weight $\gamma^t$ falls to $1/e$ at $t \approx 1/(1-\gamma)$. $\gamma{=}0.9 \to$ ~10 steps, $0.99 \to$ ~100 (the Atari default), $0.999 \to$ ~1000
- The price of long horizons: error/complexity bounds degrade polynomially in $\tfrac{1}{1-\gamma}$ — e.g. approximate value iteration turns per-step error $\epsilon$ into $O\!\big(\tfrac{\epsilon}{(1-\gamma)^2}\big)$ policy loss ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]]). Long credit assignment is intrinsically expensive
- Episodic tasks can use $\gamma = 1$ with finite horizon — the LLM post-training setting

## Why exact solutions don't scale

- **Result:** finite MDPs are solvable exactly (value/policy iteration, or a linear program) in time polynomial in $|\mathcal{S}|, |\mathcal{A}|$ ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])
- **The catch — curse of dimensionality:** $|\mathcal{S}|$ is exponential in the number of state variables (a 10×10 grid of 5 objects is fine; an image is $256^{3 \cdot HW}$). Exact DP dies immediately; all of deep RL is function approximation forced by this fact ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]])
- **Conclusion:** the map of methods = how much of $(P, R)$ you know. Known model → dynamic programming ([[Bellman Equation]]). Samples only → model-free RL ([[Temporal Difference Learning]], [[Policy Gradient]]). Learn the model first → [[Model-Based RL]]

## Extensions worth naming

- **Options / semi-MDPs**: temporally extended actions ("go to the door") with initiation sets and termination conditions; a set of options over an MDP *is* an SMDP, so value iteration and Q-learning lift to hierarchies unchanged — plus interruption and intra-option learning ([[Between MDPs and Semi-MDPs - Options (1999)|Sutton, Precup & Singh 1999]])
- **Bandits** = the 1-state MDP: no transitions, so the entire problem is [[Exploration vs Exploitation]]
- **The LLM post-training MDP**: state = prompt + tokens generated so far, action = next token, transition = deterministic append, reward = usually terminal-only (from a [[Reward Model]] or verifier), $\gamma = 1$, horizon = generation length. Deterministic dynamics + terminal reward is what makes [[RLHF]]/[[GRPO]] a structurally *easy* MDP with a *hard* credit-assignment problem

## Related

- [[Bellman Equation]] — values and optimality on top of this formalism
- [[Temporal Difference Learning]] — learning values from sampled transitions
- [[Exploration vs Exploitation]] — the bandit core of the problem

## Sources

- [[Markov Decision Processes - Puterman (1994)]] — the theory reference (lineage: Bellman 1957, Howard 1960)
- [[Dynamic Programming and Optimal Control - Bertsekas (1995)]] — curse of dimensionality, approximation bounds
- [[Reinforcement Learning - An Introduction (1998)]] — the field's framing and notation
- [[Between MDPs and Semi-MDPs - Options (1999)]] — temporal abstraction

---
Part of the RL foundations cluster — map: [[RL MOC]].
