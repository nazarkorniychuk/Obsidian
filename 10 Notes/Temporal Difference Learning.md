---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-27
aliases: [TD learning, TD, TD(0), TD(lambda), eligibility traces, bootstrapping]
---

# Temporal Difference Learning

Learn value functions from raw experience by making predictions consistent with *later predictions*, not with final outcomes. The single algorithmic idea underneath Q-learning, SARSA, actor-critic critics, and [[Generalized Advantage Estimation|GAE]].

## ⚡ Summary

| Concept | Formula / statement |
|---|---|
| TD(0) update | $V(s_t) \mathrel{+}= \alpha\,\delta_t$, $\;\delta_t = r_{t+1} + \gamma V(s_{t+1}) - V(s_t)$ |
| TD error $\delta_t$ | sampled Bellman residual; the universal learning signal |
| λ-return | $G_t^\lambda = (1-\lambda)\sum_{n\ge1} \lambda^{n-1} G_t^{(n)}$; $\lambda{=}0\to$ TD(0), $\lambda{=}1\to$ MC |
| Eligibility trace | $e_t = \gamma\lambda\, e_{t-1} + \nabla_\theta V(s_t)$; online implementation of the λ-return |
| Tabular convergence | w.p.1 (TD(λ): Dayan; Q-learning: Watkins & Dayan) |
| Linear FA, on-policy | converges w.p.1; error $\le \tfrac{1-\lambda\gamma}{1-\gamma}\times$ best-fit error (Tsitsiklis & Van Roy) |
| Deadly triad | FA + bootstrapping + off-policy ⇒ can diverge (Baird) |

## The idea and the trade

Monte Carlo waits for the episode's actual return $G_t$ (unbiased, high variance, no learning until the end). TD **bootstraps**: it treats its own next-state estimate as ground truth (biased while learning, low variance, updates every step, no model needed). [[Learning to Predict by the Methods of Temporal Differences (1988)|Sutton 1988]]:

- **Results:** TD(0) converges (in expectation) to the true conditional-expectation predictions for absorbing Markov chains with linearly independent features; on finite data (random-walk experiment) TD is *more accurate on future data than the supervised/maximum-likelihood fit* — it exploits the Markov structure MC ignores
- TD needs **less memory and peak computation** than outcome methods: incremental updates, no stored episodes
- DP–MC–TD in one line: DP bootstraps with a model; MC samples without bootstrapping; **TD samples *and* bootstraps** ([[Reinforcement Learning - An Introduction (1998)|Sutton & Barto]])

## TD(λ): the bias–variance dial

The λ-return mixes all n-step returns geometrically; eligibility traces implement it online at $O(\text{params})$ per step. **Results:** convergence in expectation for all λ ([[The Convergence of TD(lambda) for General lambda (1992)|Dayan 1992]]), later w.p.1 (Dayan & Sejnowski 1994). In practice intermediate λ (~0.9–0.97) dominates both endpoints — the same trade reappears verbatim as GAE's λ in [[PPO]].

## Function approximation: where the guarantees end

[[An Analysis of Temporal-Difference Learning with Function Approximation (1997)|Tsitsiklis & Van Roy 1997]] drew the exact boundary:

- **Positive:** linear TD(λ), trained **on-policy** along the chain's own trajectory, converges w.p.1 with error bound $\|V_{\theta^*} - V^\pi\|_D \le \tfrac{1-\lambda\gamma}{1-\gamma}\|\Pi V^\pi - V^\pi\|_D$ — larger λ provably tightens the bound
- **Negative:** update under any *other* state distribution and TD can diverge; nonlinear approximators can diverge even on-policy
- **The deadly triad** (Sutton & Barto's name): function approximation + bootstrapping + off-policy data. Any two are safe; all three can blow up. Canonical demo: Baird's 7-state counterexample, where Q-learning's values **diverge to infinity** despite an exact solution being representable ([[Off-Policy TD with Function Approximation - Precup (2001)|Precup 2001]])
- **Conclusion:** [[Deep Q-Network|DQN]]'s target networks and replay buffers are engineering patches for exactly this — freeze the bootstrap target (less bootstrapping), keep data near-on-policy (less off-policy)
- Modern quantitative rates: TD with linear FA ≈ online gradient descent, $O(1/\sqrt{T})$ with averaging, extending to Markovian sampling and TD(λ) ([[A Finite Time Analysis of Temporal Difference Learning (2018)|Bhandari 2018]]) — notable because the TD update is *not* the gradient of any fixed objective

## TD control: SARSA and Q-learning

Apply TD to $Q$ instead of $V$:

- **SARSA** (on-policy): $\delta = r + \gamma Q(s',a') - Q(s,a)$ with $a'$ actually taken — evaluates the policy being followed, exploration included
- **[[Q-Learning]]** (off-policy): $\delta = r + \gamma \max_{a'}Q(s',a') - Q(s,a)$ — evaluates the *greedy* policy while behaving arbitrarily. **Result:** converges to $Q^*$ w.p.1 given every $(s,a)$ visited infinitely often + Robbins–Monro steps ($\sum\alpha{=}\infty, \sum\alpha^2{<}\infty$) ([[Q-learning - Watkins & Dayan (1992)|Watkins & Dayan 1992]])

## The result that made TD famous

**TD-Gammon**: TD(λ) + neural network + self-play backgammon, from zero knowledge. **Results:** stronger than commercial programs and — the striking part — **better than the same network trained supervised on a massive human-expert dataset**; later versions reached world-champion level ([[Practical Issues in Temporal Difference Learning - TD-Gammon (1992)|Tesauro 1992]]). The direct ancestor of the AlphaGo/[[Monte Carlo Tree Search|AlphaZero]] self-play lineage, and a 25-years-early demo that "theoretically unsound" nonlinear TD works well in practice when data stays on-policy.

## Related

- [[Bellman Equation]] — TD = sampled Bellman backup
- [[Q-Learning]] · [[Deep Q-Network]] — TD control and its deep patches
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
