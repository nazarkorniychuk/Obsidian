---
type: source
source-type: paper
authors: [Rico Sennrich, Barry Haddow, Alexandra Birch]
year: 2016
url: https://consensus.app/papers/details/650b041a110854269891ec5580bcfad9/?utm_source=claude_desktop
doi: 10.18653/v1/p16-1162
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [BPE paper, Sennrich 2016]
---

# Neural Machine Translation of Rare Words with Subword Units (2016)

**The BPE paper** (~8,900 citations).

## Summary

Translation is open-vocabulary but models have fixed vocabularies. Solution: repurpose the **byte-pair encoding compression algorithm** as segmentation — start from characters, iteratively **merge the most frequent adjacent symbol pair**, repeat for $k$ merges; the learned merge table then segments any text into subwords. Frequent words stay whole; rare words decompose into meaningful pieces (morphemes, transliterable chunks). No out-of-vocabulary tokens, vocabulary size is a free parameter.

## Key results

- **+1.1 / +1.3 BLEU** (WMT'15 En-De / En-Ru) over the back-off dictionary baseline for rare and unseen words

## Impact

The default tokenization of the LLM era: GPT-2 introduced **byte-level BPE** (base alphabet = 256 bytes → no unknown characters ever, vocab 50,257), LLaMA used SentencePiece-BPE (32k), LLaMA-3/GPT-4-class moved to ~100–256k vocabularies. Alternatives from the same family: WordPiece (BERT), unigram-LM segmentation. Known weaknesses inherited by every LLM: digit chunking (arithmetic), character-level blindness (spelling), unequal per-language fertility.

## Concepts extracted

- [[Tokenizer]] — the core algorithm
