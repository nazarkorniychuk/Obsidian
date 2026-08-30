---
type: source
source-type: paper
authors: [Danijar Hafner, Jurgis Pasukonis, Jimmy Ba, Timothy Lillicrap]
year: 2023
url: https://consensus.app/papers/details/6885f3230a6653398151caf97055b1e7/?utm_source=claude_desktop
doi: 10.48550/arxiv.2301.04104
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [DreamerV3, Dreamer, world models, Hafner 2023]
---

# Mastering Diverse Domains - DreamerV3 (2023)

(~1,300 citations + ~500 for the *Nature* version — the flagship of the world-model line.)

## Summary

The **Dreamer** recipe, matured: learn a compact latent **world model** from experience (encode observations → predict latent dynamics and rewards), then train actor and critic **entirely inside the model's imagination** — rolling out imagined trajectories in latent space, never touching pixels during policy learning.

## Key results

- **Outperforms specialized methods across 150+ diverse tasks with a single configuration** — no per-domain tuning; robustness comes from normalization/balancing/transformation tricks
- **First algorithm to collect diamonds in Minecraft from scratch** — no human data, no curricula: farsighted sparse-reward exploration from pixels in an open world
- Lineage of results behind it: PlaNet (planning in latent space from pixels), Dreamer (backprop through imagined trajectories), DreamerV2 (first world-model agent at human-level Atari, exceeding Rainbow at matched budget)

## Impact

The strongest standing evidence that "learn the world, practice in the dream" scales to general control — and the conceptual bridge to sequence models as world models.

## Concepts extracted

- [[Model-Based RL]] — imagination training at maturity
