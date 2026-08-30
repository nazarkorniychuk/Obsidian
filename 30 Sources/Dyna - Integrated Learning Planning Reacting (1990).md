---
type: source
source-type: paper
authors: [Richard S. Sutton]
year: 1990
url: https://consensus.app/papers/details/d24bf2bf3f695d41804085a578c9565d/?utm_source=claude_desktop
doi: 10.1145/122344.122377
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Dyna, Sutton 1990]
---

# Dyna - Integrated Learning Planning Reacting (1990)

(~1,200 citations across versions — the founding architecture of model-based RL.)

## Summary

**Dyna**: interleave three processes in one loop — act in the world, learn a model of it from the experienced transitions, and *plan* by running the same RL updates (e.g. Q-learning backups) on **imagined transitions drawn from the model**. Real and simulated experience feed the identical learning rule; planning is "learning from hallucinated data."

## Key results

- On navigation tasks, a Dyna agent simultaneously learns by trial and error, learns the world model, and plans routes with it — converging in far fewer *real* steps than model-free learning alone
- Planning works even with the probabilistic, often-incorrect models that learning produces; **Dyna-Q** (the Q-learning instantiation) adapts easily to changing environments

## Impact

The conceptual template of all modern [[Model-Based RL]]: replay buffers are Dyna with a *non-parametric* model (the data itself), and Dreamer-style "training in imagination" is Dyna with a deep latent model.

## Concepts extracted

- [[Model-Based RL]] — the founding loop
