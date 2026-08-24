---
type: source
source-type: paper
authors: [Alec Radford, Karthik Narasimhan, Tim Salimans, Ilya Sutskever]
year: 2018
url: https://consensus.app/papers/details/32d2d9ba44935180b3bdc661f7ed5c4b/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [GPT-1, Radford 2018]
---

# GPT-1 - Improving Language Understanding by Generative Pre-Training (2018)

**The first GPT** (OpenAI tech report, ~15,000 citations).

## Summary

Generative pre-training of a 12-layer transformer *decoder* on unlabeled text (BooksCorpus), then discriminative fine-tuning per task with minimal architecture changes — the recipe [[BERT (2019)]] answered months later with the bidirectional encoder version, and GPT-2/3 scaled up.

## Key results

- SOTA on **9 of 12** NLU benchmarks: +8.9% commonsense reasoning (Stories Cloze), +5.7% QA (RACE), +1.5% MultiNLI

## FFN/activation specifics (why this vault cites it)

**First transformer to use GELU** (from [[Gaussian Error Linear Units (2016)]]) in its FFN, replacing ReLU — the swap BERT copied and the whole encoder-transformer era standardized on, at the unchanged dense 4× shape.

## Concepts extracted

- [[Activation Function]] — GELU's transformer adoption
- [[Feedforward Network]] — dense GELU 4× row
