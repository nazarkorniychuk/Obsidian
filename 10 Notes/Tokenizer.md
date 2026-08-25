---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [Tokenization, BPE, Byte-Pair Encoding]
---

# Tokenizer

The first step of the [[Transformer]] pipeline: convert raw text into a sequence of integer ids from a fixed vocabulary $V$. Trained *separately* from the model (frequency statistics, not gradients) — the one component of an LLM that isn't learned end-to-end, and the source of a distinctive family of failure modes.

## BPE — the dominant algorithm

([[Neural Machine Translation of Rare Words with Subword Units (2016)|Sennrich 2016]], repurposing a 1994 compression algorithm.)

1. Initialize the vocabulary with single characters (or bytes)
2. Count all adjacent symbol pairs in the corpus; **merge the most frequent pair** into a new symbol
3. Repeat for $k$ merges → vocabulary size ≈ base alphabet + $k$

Frequent words end up as single tokens; rare words decompose into meaningful subwords (`un + believ + able`); **no out-of-vocabulary tokens ever**. Original result: +1.1/+1.3 BLEU over dictionary-backoff on WMT'15. Segmentation at inference = greedily apply the learned merge table.

**Byte-level BPE** (GPT-2): base alphabet = the 256 bytes → *any* string tokenizes, no unicode edge cases. Relatives: WordPiece (BERT — likelihood-based merge criterion), unigram-LM (SentencePiece — probabilistic segmentation).

## ⚡ Vocabulary sizes — the design dial

| Model | Vocab | Style |
| --- | --- | --- |
| GPT-2 / GPT-3 | 50,257 | byte-level BPE |
| LLaMA-1/2 | 32,000 | SentencePiece BPE |
| LLaMA-3 | 128,256 | BPE (tiktoken-style) |
| Qwen / GPT-4-class | ~150–200k | byte-level BPE |
| Gemma | 256,000 | SentencePiece |

**The tradeoff:** larger $V$ → fewer tokens per text (↓ sequence length → ↓ attention cost, ↑ effective context) and better multilingual coverage — but [[Embedding]]/[[Unembedding]] matrices grow as $V \times d$ (Gemma-7B: 256k × 3072 ≈ 786M params in embeddings alone) and softmax cost grows with $V$. The drift from 32k → 128k+ tracks models becoming larger (embedding share shrinks) and more multilingual (fertility matters more).

**Compression = quality:** for fixed vocab budget, segmentations that cover text in *fewer tokens* translate measurably better — sequence length is the real objective, not linguistic elegance.

## Failure modes (inherited by every LLM)

- **Arithmetic:** numbers chunk arbitrarily (`2023` → `20`+`23` or one token by frequency) — digit alignment across operands is destroyed; a root cause of LLM arithmetic errors (modern fix: force single-digit tokens)
- **Spelling/character blindness:** the model never sees characters — "how many r's in strawberry" fails because `strawberry` is 1–3 opaque tokens
- **Multilingual inequity (fertility):** BPE trained on English-heavy corpora segments other scripts into many more tokens per word — same text costs more context, compute, and API dollars in Thai than English
- **Trailing-space/glitch tokens:** artifacts of frequency-trained vocabularies (under-trained tokens with pathological embeddings)

## The token-free alternative

Feed raw **UTF-8 bytes** (vocab 256): ByT5 is competitive with token-level T5 at matched parameters, **markedly more robust to noise and better at spelling-sensitive tasks**, any language out of the box ([[ByT5 (2021)|Xue 2021]]). The price: ~4× longer sequences → quadratic attention tax — which is why the industry grew vocabularies instead. Byte-level and hierarchical-patch models remain the standing escape hatch from tokenizer pathology.

## Hardware note

Tokenization itself is trivial CPU work; the hardware footprint is indirect — $V$ sets embedding/unembedding parameter count and the final $d \times V$ logit GEMM + softmax per decoded token (at $V{=}256$k this matmul is non-trivial), which is why serving stacks fuse and shard the LM head.

## Related

- Feeds the [[Embedding]]; mirrored at the output by the [[Unembedding]] over the same vocabulary
- First step of the [[Transformer]] forward pass
- Vocabulary size trades against [[Attention Mechanism]] sequence cost

## Sources

- [[Neural Machine Translation of Rare Words with Subword Units (2016)]] — BPE
- [[ByT5 (2021)]] — the token-free case
- [[Language Models are Few-Shot Learners - GPT-3 (2020)]], [[BERT (2019)]], [[T5 - Exploring the Limits of Transfer Learning (2019)]] — adopter configs

---
Part of the [[Transformer]] cluster
