---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [Bellman equations, value function, value iteration, policy iteration, dynamic programming]
---

# Bellman Equation

> **The foundations pipeline — who solves what.** [[Markov Decision Process]] poses the problem → **this note solves it *on paper* when the rulebook $(P,R)$ is known — *planning*: no agent, no data, no exploration; everything knowable is already in the tables, we only compute consequences** → [[Temporal Difference Learning]] solves it *from experience* when the rulebook is missing (learning) → [[Exploration vs Exploitation]] supplies that experience.

## Why value functions exist

The [[Markov Decision Process|MDP]] objective is the expected return $\mathbb{E}[G_t]$ — a sum over the *entire future*. To act well you constantly need to compare futures: "if I land in state $s$, how good is my life from there?" The **value function** compresses that infinite future into one number per state:

$$V^\pi(s) = \mathbb{E}_\pi[G_t \mid s_t = s] \qquad \text{"expected total reward from } s \text{, if I follow policy } \pi \text{ afterwards"}$$

Computed naively, this is an expectation over infinitely many infinitely-long trajectories — hopeless. The Bellman equation is the observation that makes it computable at all.

## What the Bellman equation is, concretely

**Definition.** The Bellman equation is a **self-consistency condition on a value function**: the value of a state must equal the expected one-step reward plus γ times the value of the state you land in,

$$V(s) \;=\; \mathbb{E}\big[\, \underbrace{r_{t+1}}_{\text{one step of reward}} +\; \gamma \underbrace{V(s_{t+1})}_{\text{value of wherever you land}} \;\big|\; s_t = s \,\big] \qquad \text{for every state } s.$$

Three things to be precise about:

- **It's a system, not a formula.** One equation *per state*; the unknown is the entire table $V$. The equation doesn't hand you any value directly — it *constrains neighboring values to agree with each other*, and the value function is the (unique, see contraction below) table satisfying all $|\mathcal{S}|$ constraints simultaneously
- **Where it comes from:** the return telescopes, $G_t = r_{t+1} + \gamma\, G_{t+1}$ — total future reward = next reward + discounted total after that. Take the expectation of both sides conditioned on $s_t = s$, and the definition $V(s) = \mathbb{E}[G_t \mid s_t = s]$ turns into the equation above. No approximation anywhere — the infinite horizon is hiding inside the $V(s_{t+1})$ term
- **Why it matters:** the definition of $V$ averages over infinitely many, infinitely long futures; the Bellman equation pins down the *same numbers* using only **one step of lookahead**. A finite system of equations replaces an infinite simulation. That swap is what "dynamic programming" means, and every value-based RL method inherits it

"The" Bellman equation is really a family: fill in *how actions get chosen* and the expectation becomes explicit sums. Choosing actions by a given policy $\pi$ yields the **expectation equation**:

$$V^\pi(s) = \underbrace{\sum_a \pi(a \mid s)}_{\text{actions } \pi \text{ might take}} \underbrace{\sum_{s'} P(s' \mid s,a)}_{\text{where the world lands}} \big[\underbrace{r(s,a)}_{\text{paid now}} + \gamma\, \underbrace{V^\pi(s')}_{\text{everything after}}\big]$$

Choosing the *best* action instead ($\max_a$ in place of $\sum_a \pi$) yields the **optimality equation** — the full family of four is next.

## The four equations

Two axes: value of *states* ($V$) vs *state–action pairs* ($Q$), and value of *a given policy* vs *the best possible* ($^*$):

| Object | Meaning | Bellman form |
|---|---|---|
| $V^\pi(s)$ | how good is $s$ under $\pi$ | $V^\pi(s) = \sum_a \pi(a \mid s) \sum_{s'} P(s' \mid s,a)\,[r + \gamma V^\pi(s')]$ |
| $Q^\pi(s,a)$ | how good is doing $a$ in $s$, then following $\pi$ | $Q^\pi = r(s,a) + \gamma \sum_{s'} P \sum_{a'} \pi(a' \mid s')\, Q^\pi(s',a')$ |
| $V^*(s)$ | how good is $s$ if you play perfectly | $V^*(s) = \max_a \sum_{s'} P(s' \mid s,a)\,[r + \gamma V^*(s')]$ |
| $Q^*(s,a)$ | how good is $a$ in $s$, then perfect play | $Q^* = r(s,a) + \gamma \sum_{s'} P\, \max_{a'} Q^*(s',a')$ |

### Two different problems hide in that table

The table mixes two *tasks*, and everything downstream depends on telling them apart:

- **Policy evaluation** — someone hands you a specific policy $\pi$ ("always move toward the goal") and asks: *how good is it?* Compute the table $V^\pi(s)$ for all $s$. You are grading given behavior, not improving it
- **Optimal control** — nobody hands you anything. Find the best achievable values $V^*(s) = \max_\pi V^\pi(s)$ and a policy that attains them. This is the actual goal of RL; evaluation is its subroutine

**Evaluation is a linear system.** In the $V^\pi$ equation the unknowns are the $|\mathcal{S}|$ numbers $V^\pi(s)$, and each appears only multiplied by known constants ($\gamma \times$ probabilities) — never inside a max or a product with another unknown. So it's $|\mathcal{S}|$ linear equations in $|\mathcal{S}|$ unknowns. Tiny worked example — two states, $\gamma = 0.5$, one action per state (that's the "fixed policy"): from $A$ you get reward 3 and land in $B$; from $B$ reward 0 and back to $A$:

$$V(A) = 3 + 0.5\,V(B), \qquad V(B) = 0 + 0.5\,V(A)$$

Substitute: $V(A) = 3 + 0.25\,V(A) \Rightarrow V(A) = 4,\; V(B) = 2$. Done — no learning, no iteration, just algebra. In general, stacking the equations into vectors ($P_\pi$ = transition matrix under $\pi$, $r_\pi$ = expected rewards):

$$V^\pi = r_\pi + \gamma P_\pi V^\pi \quad\Longrightarrow\quad V^\pi = (I - \gamma P_\pi)^{-1} r_\pi$$

— one matrix inversion, $O(|\mathcal{S}|^3)$, the inverse existing because $\gamma < 1$. This exact solve is the "evaluate" half of policy iteration below.

**Optimality is not, because of the max.** Give state $A$ a second action: $a_1$ = reward 3, go to $B$ (as before); $a_2$ = reward 1, stay in $A$. The optimality equation now reads

$$V^*(A) = \max\big\{\underbrace{3 + 0.5\,V^*(B)}_{a_1},\; \underbrace{1 + 0.5\,V^*(A)}_{a_2}\big\}, \qquad V^*(B) = 0.5\,V^*(A)$$

You cannot solve this as a linear system, because **you don't know which branch of the max is active until you already know the values** — and the values depend on the branch. The only way through is guess-and-verify: *assume* $a_1$ wins → linear solve gives $V^*(A) = 4, V^*(B) = 2$ → check the other branch: $1 + 0.5 \cdot 4 = 3 < 4$ ✓, guess confirmed. With 2 actions in each of $K$ states there are $2^K$ possible branch patterns — each pattern *is* a candidate policy, and you can't enumerate them. Value iteration and policy iteration (below) are the two systematic ways to navigate this: VI iterates the max-equation directly; PI is exactly the guess-a-branch-pattern → linear-solve → re-guess loop from this example, automated. The circularity — *values need the policy, the policy needs the values* — is the mathematical content of "finding optimal behavior is hard." Saving grace: the max is a tame nonlinearity (piecewise-linear, monotone, still a γ-contraction), so the fixed-point machinery below survives it.

**Why algorithms learn $Q$ rather than $V$:** given $Q^*$, acting optimally is a lookup — $\pi^*(s) = \arg\max_a Q^*(s,a)$. Given only $V^*$, choosing an action requires imagining outcomes ("which $a$ leads to high-value states?"), i.e. it requires knowing $P$. $Q$ has the one-step lookahead *baked in* — that's the entire reason model-free control works.

One derived quantity worth naming now: the **advantage** $A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)$ — "how much better is action $a$ than what $\pi$ would do on average here." It's zero-mean under $\pi$, which makes it the natural low-variance learning signal — the currency of all [[Policy Gradient|policy-gradient]] methods.

## Why a solution exists at all: the contraction

The Bellman optimality equation is $|\mathcal{S}|$ coupled nonlinear equations. Before trying to solve it, three questions need answers: does a solution exist? Is it unique? Can we compute it? One property answers all three.

**The Bellman operator.** Define $T$ as a machine that eats an entire value table and outputs a new one: for every state, *pretend the input table is correct about the future* and recompute the present by one step of lookahead:

$$(TV)(s) = \max_a \sum_{s'} P(s' \mid s,a)\,[\,r + \gamma V(s')\,]$$

The input $V$ can be garbage — $T$ doesn't care. Two observations connect $T$ to the problem:

- A table with $TV = V$ (recomputing changes nothing — a **fixed point**) is *exactly* a table satisfying the optimality equation. "Solve the Bellman equation" = "find the fixed point of $T$"
- Measure distance between tables by their **largest per-state disagreement**: $\|V - U\|_\infty = \max_s |V(s) - U(s)|$ (the sup-norm)

**The key property** ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]): $T$ is a **γ-contraction**,

$$\|TV - TU\|_\infty \le \gamma\,\|V - U\|_\infty$$

*Why:* compare $(TV)(s)$ and $(TU)(s)$. The reward terms are identical — the two tables only enter through $\gamma V(s')$ vs $\gamma U(s')$, averaged over probabilities that sum to 1. An average of disagreements can't exceed the largest disagreement, and the γ in front shrinks it: the outputs differ by at most $\gamma \times$ the biggest input disagreement. **Every application of $T$ shrinks any disagreement by at least a factor γ.**

**What contraction buys — all three answers at once:**

- **Uniqueness:** two different fixed points would have to be γ-closer to each other than they are to themselves — impossible unless their distance is 0. At most one solution
- **Existence + computability:** start from *any* table $V_0$ and iterate $V_{k+1} = TV_k$. Since $T$ shrinks the distance to the (eventual) fixed point by γ per step, the iterates converge — and the limit is $V^*$. Watch it run on the note's 2-state example ($a_1$: reward 3, $A{\to}B$; $a_2$: reward 1, stay; $B$: reward 0, ${\to}A$; γ = 0.5), starting from all-zeros:

| sweep | $V(A)$ | $V(B)$ | worst error vs $(4, 2)$ |
|---|---|---|---|
| 0 | 0 | 0 | 4 |
| 1 | 3 | 0 | 2 |
| 2 | 3 | 1.5 | 1 |
| 3 | 3.75 | 1.5 | 0.5 |
| 4 | 3.75 | 1.875 | 0.25 |
| … | → 4 | → 2 | halves each sweep = $\gamma^k$ |

- **Speed:** the table shows the general law, $\|V_k - V^*\|_\infty \le \gamma^k \|V_0 - V^*\|_\infty$. Precision $\epsilon$ needs $k \approx \tfrac{\ln(1/\epsilon)}{1-\gamma}$ sweeps. Here γ = 0.5 → error halves per sweep; at γ = 0.99 each sweep removes only 1% of the error (~460 sweeps for 1% precision) — the $\tfrac{1}{1-\gamma}$ horizon tax from the [[Markov Decision Process|MDP note]], now as compute
- **Robustness:** if you stop early / approximate, acting greedily on a $V$ that's off by $\epsilon$ costs at most $\tfrac{2\gamma\epsilon}{1-\gamma}$ in policy quality ([[Dynamic Programming and Optimal Control - Bertsekas (1995)|Bertsekas 1995]]) — value errors don't explode into arbitrarily bad behavior, but the horizon amplifies them

## The two classical solvers

**Value iteration (VI)** is the convergence proof used as an algorithm: iterate $V_{k+1} = TV_k$ until successive tables barely change, then act greedily. The table above *is* VI running. Simple and global, but the rate is the contraction rate γ — painfully slow when γ ≈ 1 (long horizons).

**Policy iteration (PI)** exploits the split from the [evaluation vs optimality section](#two-different-problems-hide-in-that-table): evaluation is cheap (linear), so use it as a subroutine. Alternate:

1. **Evaluate:** solve the linear system for $V^{\pi_k}$ — exact values of the *current* policy
2. **Improve:** in every state, switch to the action with the best one-step lookahead under those values: $\pi_{k+1}(s) = \arg\max_a \sum_{s'}P(s'\mid s,a)[r + \gamma V^{\pi_k}(s')]$

On the running example, starting from the *wrong* policy π₀ = "stay in A": evaluate → $V(A) = 1 + 0.5V(A) \Rightarrow V(A)=2,\ V(B)=1$; improve → compare $Q(A,a_1) = 3 + 0.5{\cdot}1 = 3.5$ vs $Q(A,a_2) = 2$ → switch to $a_1$; evaluate → $(4,2)$; improve → no switch. **Converged exactly, in 2 rounds.** Why each round can only help (*policy improvement theorem*): the evaluation is honest about $\pi_k$, so switching to actions that look strictly better under it makes the new policy at least as good *in every state*; with finitely many policies and no repeats, PI terminates in finite time ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]).

**Why PI is so much faster than VI:** each policy pins down one linear "branch" of the piecewise-linear optimality equation, and PI's evaluate step solves that branch *completely* in one shot — then improvement jumps branches. That is structurally the **Newton method** (solve the local linearization exactly, jump, re-linearize): PI is precisely semismooth Newton on the Bellman equation and inherits **local quadratic convergence** — error *squares* per round near the solution — while VI takes fixed-size γ-steps ([[Dynamic Programming as Semismooth Newton (2022)|Gargiani 2022]]). PI : VI :: Newton : gradient descent; the empirical "PI needs ~5 rounds on huge MDPs" has a textbook explanation.

**The third route — linear programming:** $\min_V \sum_s V(s)$ subject to $V \ge TV$. Any table with $V \ge TV$ overestimates $V^*$ everywhere (backing up an overestimate keeps it an overestimate), so the smallest such table *is* $V^*$ — and the max disappears into $|\mathcal{A}|$ linear constraints per state. The dual LP optimizes over **occupancy measures** (expected discounted visit-counts per state–action) — the language offline-RL and constrained-RL theory is written in ([[Markov Decision Processes - Puterman (1994)|Puterman 1994]]).

Step back and notice what *kind* of activity this whole section was: **planning, not learning**. Both solvers consume the tables $P$ and $R$; no action was ever taken, no transition experienced, and "exploration" doesn't even parse as a concept — there is nothing left to discover, only consequences to compute. RL proper begins when the tables are gone: [[Temporal Difference Learning]] rebuilds these exact backups from sampled experience, and [[Exploration vs Exploitation]] makes sure the experience covers what matters.

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
