---
type: source
source-type: paper
authors: [Samira Abnar, et al. (Apple)]
year: 2025
url: https://consensus.app/papers/details/0cbced9e34685d9b99d9dd327cf71511/?utm_source=claude_desktop
doi: 10.48550/arxiv.2501.12370
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Abnar 2025, optimal sparsity]
---

# Parameters vs FLOPs - Optimal Sparsity for MoE (2025)

*Full title: "Parameters vs FLOPs: Scaling Laws for Optimal Sparsity for Mixture-of-Experts Language Models."*

## Summary

Model capacity has two axes — parameter count and FLOPs per example. Sparsity (fraction of *inactive* parameters) is the knob that trades between them. This paper sweeps sparsity levels under fixed constraints to find the optimum.

## Key results

- **An optimal sparsity level exists** under each constraint regime (fixed params, fixed training compute) — improving both training efficiency and final performance; neither dense (0% sparse) nor maximally sparse is right
- The optimum shifts with the constraint: compute-limited training favors higher sparsity (more total params at same FLOPs)
- Effects differ between pre-training loss and downstream few-shot metrics — sparsity choices don't transfer blindly across objectives

## Concepts extracted

- [[Mixture of Experts]] — active/total ratio law
