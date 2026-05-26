# 001. LLM General Index

Related pages:

- [000. Glossary](./000-glossary.md)
- [002. RAG](./002-rag.md)

---

## 1. How Tokens Are Created Before LLM Training

Related glossary terms:

- [Token](./000-glossary.md#token)
- [Tokenizer](./000-glossary.md#tokenizer)
- [Vocabulary](./000-glossary.md#vocabulary)
- [BPE](./000-glossary.md#bpe)
- [SentencePiece](./000-glossary.md#sentencepiece)

Tokens are created before model training by training a tokenizer on a large text corpus.

The tokenizer learns a fixed vocabulary of common chunks, such as full words, word parts, punctuation, spaces, bytes, and special control tokens. After that, the LLM is trained to read and predict token IDs rather than raw text.

Most modern LLMs use a tokenizer method related to BPE, byte-level BPE, SentencePiece, or Unigram tokenization.

Example:

```text
unbelievable -> un + believ + able
```

A common word may be one token. A rare word, new word, typo, or technical term may be split into several smaller tokens.

---

## 2. What The Set Of Tokens Is Called

Related glossary terms:

- [Vocabulary](./000-glossary.md#vocabulary)
- [Vocabulary Size](./000-glossary.md#vocabulary-size)
- [Token ID](./000-glossary.md#token-id)

The correct general term is **vocabulary**.

More precise terms are:

- **Tokenizer vocabulary**: The full set of tokens known by a tokenizer.
- **Token vocabulary**: Another clear phrase for the same idea.
- **Vocabulary size**: The number of tokens in that set.

In LLM engineering, saying "this model has a 128k vocabulary" usually means the tokenizer has about 128,000 possible token IDs.

---

## 3. Are Tokens Reused Or Recreated For Each New Model?

Related glossary terms:

- [Token](./000-glossary.md#token)
- [Tokenizer](./000-glossary.md#tokenizer)
- [Vocabulary](./000-glossary.md#vocabulary)

Technically, both approaches are possible.

A company can reuse an existing tokenizer for a new model, or it can train a new tokenizer and vocabulary for a new model generation.

In regular practice, companies often reuse a tokenizer within a model family because it keeps compatibility between models, tools, prompts, datasets, fine-tuning pipelines, and evaluation systems.

But companies may create a new tokenizer when a new generation needs better multilingual coverage, better code handling, better compression, longer context efficiency, or a different chat/control-token format.

So the practical pattern is:

1. Reuse the tokenizer for nearby model versions inside the same family.
2. Redesign or retrain the tokenizer for larger generation changes.
3. Keep old tokenizers available because old models still depend on them.

---

## 4. Vocabulary Size In Leading Public LLMs

Related glossary terms:

- [Vocabulary Size](./000-glossary.md#vocabulary-size)
- [Token ID](./000-glossary.md#token-id)
- [Context Size](./000-glossary.md#context-size)

Vocabulary size means how many possible token IDs the model can represent. This is different from context size, which means how many tokens the model can read in one prompt.

Approximate public vocabulary sizes:

| Model family | Approximate vocabulary size | Notes |
| --- | ---: | --- |
| OpenAI GPT-4o class | About 200k | Public OpenAI tooling uses the `o200k_base` tokenizer family. |
| OpenAI GPT-4 / GPT-3.5 era | About 100k | Public OpenAI tooling uses the `cl100k_base` tokenizer family. |
| Meta Llama 3 / 3.1 | 128k | Meta publicly describes Llama 3 as using a 128K-token vocabulary. |
| Mistral modern Tekken tokenizer | 131k | Mistral documents a 131k vocabulary for Tekken. |
| Qwen | About 151k | Qwen documentation describes 151,643 regular BPE tokens, plus control tokens. |
| DeepSeek-V3 | About 128k to 129k | Public model configs and reports place it around this size. |
| Google Gemma | About 256k to 262k | Public Gemma configs use a large vocabulary in this range. |
| Google Gemini | Not fully public | Google exposes token counting, but not always the exact frontier tokenizer vocabulary. |
| Anthropic Claude | Not fully public | Anthropic exposes token counting, but not the full public tokenizer specification. |

These numbers are approximate because some companies publish exact tokenizer files while others expose only token-counting APIs.

---

## 5. Does A Bigger Vocabulary Mean A Better Model?

Related glossary terms:

- [Vocabulary](./000-glossary.md#vocabulary)
- [Embedding](./000-glossary.md#embedding)

Not automatically.

A bigger vocabulary can be useful, but it is not direct evidence that a model is more intelligent, more capable, or more advanced overall.

A larger vocabulary can help with:

1. Better compression for many languages.
2. Fewer tokens for some words, names, code, or symbols.
3. More efficient handling of multilingual text.
4. Less fragmentation of rare words or technical terms.

But a larger vocabulary can also have tradeoffs:

1. Larger embedding and output layers.
2. More parameters spent on token representations.
3. More complexity in tokenizer design.
4. Possible inefficiency if many tokens are rarely used.

Model quality depends much more on architecture, training data, training compute, post-training, reinforcement learning, evaluation, tooling, and inference systems.

So a model with a 200k vocabulary is not automatically twice as advanced as a model with a 100k vocabulary.

The vocabulary is like the alphabet or dictionary available to the model. A larger dictionary can help, but it does not by itself make the writer smarter.
