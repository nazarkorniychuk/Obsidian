---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [MDP, MDPs, Markov decision processes]
---

# Markov Decision Process

## Why this formalism exists

Supervised learning gets labeled examples: input → correct output. But how do you learn to *act* — play backgammon, control a robot, generate a good answer — when nobody ever shows you the correct action? All you get is a score after the fact. That is the reinforcement-learning problem, and it is harder than supervised learning in three specific ways:

1. **Evaluation, not instruction** — feedback says "you won" or "7/10", never "the right move was 13-9"
2. **Delayed consequences** — the move that lost the game may have happened 30 moves before the loss; figuring out *which* past action deserves blame is the **credit-assignment problem**
3. **You generate your own data** — your actions decide which situations you see next; a policy that never explores never even *sees* the states where the reward is ([[Exploration vs Exploitation]])

The MDP is the minimal mathematical object that captures all three. Everything in the [[RL MOC|RL cluster]] is an algorithm for some MDP.

> **The foundations pipeline — who solves what.** **This note *poses* the problem** → [[Bellman Equation]] solves it *on paper* when the rulebook $(P, R)$ is known (**planning**) → [[Temporal Difference Learning]] solves it *from experience* when the rulebook is missing (**learning**) → [[Exploration vs Exploitation]] *supplies the experience*: TD only learns about what the policy actually visits, so the visits themselves must be engineered.

## The formalism, piece by piece

An MDP is a tuple $(\mathcal{S}, \mathcal{A}, P, R, \gamma)$. Concretely, with two running examples:

| Object | Meaning | Backgammon | LLM generation |
|---|---|---|---|
| $\mathcal{S}$ — states | a full snapshot of the situation | board + dice | prompt + tokens so far |
| $\mathcal{A}$ — actions | what the agent can do | legal moves | next token |
| $P(s' \mid s, a)$ — transitions | where the world lands after action $a$ in state $s$; a *probability* because the environment has its own randomness | opponent's reply + next dice roll | deterministic: append the token |
| $R(s,a)$ — reward | one number per step; the only feedback | +1 on winning, 0 otherwise | usually 0 until the end, then a [[Reward Model]]/verifier score |
| $\gamma \in [0,1)$ — discount | how much the future counts (next section) | — | γ = 1, finite episodes |

**The interaction loop.** At each step $t$: the agent observes $s_t$, samples an action from its **policy** $a_t \sim \pi(a \mid s_t)$, the environment draws $s_{t+1} \sim P(\cdot \mid s_t, a_t)$ and pays reward $r_{t+1}$. The recorded sequence $s_0, a_0, r_1, s_1, a_1, r_2, \dots$ is a **trajectory** (an **episode** if it ends). The policy $\pi$ — a rule assigning action probabilities to every state — is *the thing being learned*; it plays the role that "the weights" play in supervised learning.

**The objective.** The agent maximizes the **return**, the discounted sum of all future reward:

$$G_t = r_{t+1} + \gamma\, r_{t+2} + \gamma^2 r_{t+3} + \cdots = \sum_{k=0}^{\infty}\gamma^k r_{t+k+1}, \qquad \text{goal:}\;\; \max_\pi\; \mathbb{E}_\pi\!\left[G_t\right]$$

Two deliberate choices in that line. It's a **sum** because we care about total outcome, not the next step — maximizing immediate reward is exactly the greedy failure mode (sacrificing a piece *now* to win *later* must be expressible). And it's an **expectation** because dice, opponents, and sampling make any single trajectory random — the policy can only be good *on average*. The value of being in a state under this objective, $V^\pi(s) = \mathbb{E}[G_t \mid s_t = s]$, is the subject of [[Bellman Equation]].

## The discount factor γ is a horizon knob

Why multiply future rewards by $\gamma^k$ at all? Three stacked reasons:

- **Math:** an infinite sum of rewards can diverge; with $\gamma < 1$ the return is bounded, $|G| \le r_{\max}/(1-\gamma)$, and the Bellman operator becomes a **γ-contraction** — the mechanism behind every convergence proof in the cluster ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])
- **Modeling:** $\gamma^t$ decays exponentially, so the agent effectively cares about the next $\approx \tfrac{1}{1-\gamma}$ steps (where the weight has fallen to $1/e$). $\gamma{=}0.9$ → ~10 steps, $0.99$ → ~100 (the Atari default), $0.999$ → ~1000. Choosing γ *is* choosing how far ahead the agent plans:

![[mdp-discount-horizon.png|460]]

- **Cost:** long horizons are intrinsically expensive — approximation error $\epsilon$ per step compounds into $O\!\big(\tfrac{\epsilon}{(1-\gamma)^2}\big)$ policy loss, and iteration counts scale with $\tfrac{1}{1-\gamma}$ ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]]). Credit assignment over 1000 steps costs more than over 10, and the bounds say so explicitly

Tasks with a natural ending (a game, one generated response) can skip discounting entirely: $\gamma = 1$ over a finite episode — the LLM post-training setting.

## The Markov property is the load-bearing assumption

$P(s_{t+1} \mid s_t, a_t) = P(s_{t+1} \mid s_0, a_0, \dots, s_t, a_t)$ — the state is a **sufficient statistic of history**: for predicting the future, knowing more of the past adds nothing. Markov-ness is a property of your *state design*, not of the world: a ball's position alone is not Markov (rising vs falling), (position, velocity) is — history gets absorbed into the state.

**Result** ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]): for finite discounted MDPs an optimal policy always exists that is

- **stationary** — same rule at every timestep, $\pi(a\mid s)$ doesn't depend on $t$. (Holds because the infinite discounted future looks identical from $s$ at any time; *fails* for finite horizons, where time-left changes the best action)
- **deterministic** — one fixed action per state, no randomization. (Randomizing averages Q-values; an average never beats $\arg\max_a Q^*(s,a)$)

So the search space collapses from all history-dependent randomized rules to the **finite set of $|\mathcal{A}|^{|\mathcal{S}|}$ lookup tables** — why policy iteration can terminate finitely at all.

**POMDP** (partially observable MDP): you don't see the state $s_t$, only an observation $o_t \sim O(\cdot\mid s_t)$ — a camera frame, not the world. Different states can emit the same observation (*aliasing*), so the future now genuinely depends on history. The fix is the **belief state** $b_t(s) = P(s_t{=}s \mid o_{1:t}, a_{1:t})$ — a probability distribution over states, updated by Bayes each step. That makes it an MDP again, but over a *continuous* space of distributions: qualitatively harder (exact planning is PSPACE-hard vs polynomial for MDPs). Deep agents approximate the belief cheaply: **frame-stacking** (4 Atari frames ⇒ velocity recoverable) or **recurrence** (hidden state = learned compressed history).

## Solving an MDP, and why exact solutions don't scale

"Solving" = finding $\pi^*$, the policy maximizing expected return from every state.

- **Result:** for finite MDPs with *known* $P$ and $R$, this is a solved problem — value iteration / policy iteration ([[Bellman Equation]]) find $\pi^*$ in time polynomial in $|\mathcal{S}|, |\mathcal{A}|$ ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]])

### The curse of dimensionality

"Polynomial in $|\mathcal{S}|$" sounds fine until you count what $|\mathcal{S}|$ *is*. A state is usually a **vector of variables** — and the state count multiplies across them: $k$ variables with $v$ values each make $v^k$ states. **The state space grows exponentially in the number of variables describing it.** That's the curse (the name is Bellman's own, coined in 1957):

- 20 binary features → $2^{20} \approx$ 1M states — already an uncomfortable table
- Backgammon: ~$10^{20}$ board states
- An Atari screen, 84×84×4 pixels of 256 shades: $256^{28{,}224}$ possible states — more than atoms in the universe, overwhelmingly never visited twice

Every exact method above stores and sweeps *a table with one entry per state* — dead on arrival for anything described by more than a handful of variables. The consequences ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]]):

- **All of deep RL is function approximation forced by this single fact:** replace the table with a network $V_\theta(s)$ / $Q_\theta(s,a)$ that *generalizes* — similar states share estimates, so what you learn in one state transfers to the astronomically many you'll never see
- The price of that generalization is the loss of the tabular convergence guarantees — the [[Temporal Difference Learning|deadly triad]] story
- Same curse, other victims: it's why exact [[Bellman Equation|policy evaluation]]'s $O(|\mathcal{S}|^3)$ solve is theoretical only, and why the POMDP belief simplex above is intractable

- **The map of methods = what you know about $(P, R)$:**
	- model known → dynamic programming ([[Bellman Equation]])
	- samples only → model-free RL: learn values ([[Temporal Difference Learning]], [[Q-Learning]]) or the policy directly ([[Policy Gradient]])
	- learn the model, then plan → [[Model-Based RL]], [[Monte Carlo Tree Search]]

## Special cases worth naming

- **Bandit** = the 1-state MDP: actions, rewards, no transitions — the entire difficulty reduces to [[Exploration vs Exploitation]], which is why bandit theory is where exploration is understood cleanly
- **Options / semi-MDPs**: temporally extended actions ("go to the door") with termination conditions; a set of options over an MDP *is* a semi-MDP, so value iteration and Q-learning lift to hierarchies unchanged ([[Between MDPs and Semi-MDPs - Options (1999)|Sutton, Precup & Singh 1999]])
- **The LLM post-training MDP**: state = prompt + tokens generated so far (the state *is* the history → trivially Markov), action = next token, transition = deterministic append, reward = terminal-only, $\gamma = 1$. Deterministic dynamics + sparse terminal reward = a structurally *easy* MDP with a *hard* credit-assignment problem — the framing behind [[RLHF]] and [[GRPO]]

## Related

- [[Bellman Equation]] — values and optimality on top of this formalism
- [[Temporal Difference Learning]] — learning values from sampled transitions
- [[Exploration vs Exploitation]] — the bandit core of the problem

## Sources

- [[Markov Decision Processes - Puterman (1994)]] — the theory reference (lineage: Bellman 1957, Howard 1960)
- [[Dynamic Programming and Optimal Control - Bertsekas (1995)]] — curse of dimensionality, error bounds
- [[Reinforcement Learning - An Introduction (1998)]] — the agent–environment framing and notation
- [[Between MDPs and Semi-MDPs - Options (1999)]] — temporal abstraction

---
Part of the RL foundations cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
