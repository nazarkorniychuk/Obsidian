---
type: source
source-type: book
authors: [Martin L. Puterman]
year: 1994
url: https://consensus.app/papers/details/d70400d5673b5f7986ec080db4812e41/?utm_source=claude_desktop
doi: 10.2307/2291177
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-27
aliases: [Puterman, Puterman 1994]
---

# Markov Decision Processes - Puterman (1994)

(~14,300 citations — *the* mathematical reference for MDP theory.)

## Summary

The rigorous, unified treatment of Markov decision processes: discrete-time models with finite/countable/arbitrary state spaces, organized around optimality criteria and the **Bellman (optimality) equation** as the common framework. Covers value iteration, policy iteration, modified policy iteration, linear-programming formulations, and average-reward / discounted / total-reward criteria in theorem-proof form.

## Key results (as used in the vault)

- For a **finite discounted MDP**, an optimal policy always exists that is **stationary and deterministic** — searching over history-dependent randomized policies buys nothing
- The Bellman operator is a **γ-contraction**; value iteration converges geometrically at rate $\gamma^k$; policy iteration terminates finitely on finite MDPs
- Discounted, average-reward, and total-reward criteria need genuinely different theory (average-reward requires chain-structure conditions)

## Impact

The theory floor under all of RL: every convergence proof for [[Temporal Difference Learning|TD]]/Q-learning leans on the contraction and fixed-point machinery formalized here. MDP theory itself traces to Bellman (1957) and Howard (1960); Puterman is the modern codification.

## Concepts extracted

- [[Markov Decision Process]] — the formalism
- [[Bellman Equation]] — contraction, VI/PI convergence
