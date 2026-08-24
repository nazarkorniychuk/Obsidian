---
type: source
source-type: paper
authors: [DeepSeek-AI]
year: 2024
url: https://consensus.app/papers/details/dd2d38b767f3582c93820157007e765b/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [DeepSeek-V3, DSv3]
---

# DeepSeek-V3 Technical Report (2024)

**The current open-weights MoE flagship** (~3,900 citations).

## Summary

**671B total / 37B active** parameters per token. Architecture: DeepSeekMoE (fine-grained experts — 256 routed + shared experts, top-8 — for better specialization than few-big-experts designs) + Multi-head Latent Attention (MLA, compresses the KV cache into latent vectors). Pioneers **auxiliary-loss-free load balancing** (bias-adjustment instead of the balancing losses used since [[Outrageously Large Neural Networks (2017)|Shazeer 2017]]) and multi-token prediction training.

## Key results

- Pre-trained on 14.8T tokens for only **2.788M H800 GPU-hours** (~$5.6M at list price) — frontier quality at a fraction of assumed cost
- Outperforms all open-source models; comparable to leading closed models of its time
- Training was stable end-to-end: no irrecoverable loss spikes, no rollbacks

## Concepts extracted

- [[Mixture of Experts]] — fine-grained + shared experts, loss-free balancing
- [[Feedforward Network]] — the 671B/37B active-vs-total data point
- [[Multi-Head Latent Attention]] (future note)
