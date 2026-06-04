Path: 001-general-index.md

# 001. LLM General Index

This repository is the knowledge base root.

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **002** | [Glossary](./002-glossary.md) | Terms and two-way links. |
| **003** | [Data Collection For LLM Training](./003-data-collection.md) | How internet, code, and other data are collected, filtered, and used for training corpora. |
| **004** | [Tokens](./004-tokens.md) | Tokens, tokenizers, vocabulary, tokenization algorithms. |
| **005** | [What An LLM Model Is Built From And What Files Store It](./005-model-files.md) | What physical parts a saved LLM contains and which files store them. |
| **006** | [ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md) | Maps classic ML terms such as sample, label, target, and prediction to LLM training terms. |
| **007** | [Training](./007-training.md) | Forward calculation flow and model training notes. |
| **008** | [Training Tables in LLMs](./008-training-tables-in-llms.md) | How trainable matrices, gradients, and backpropagation work together during LLM training. |
| **010** | [Embedding Output Shape and Positional Information](./010-embedding-output-shape-and-positional-information.md) | Why embedding lookup produces a T x D matrix and how models add token-position information. |
| **011** | [Attention Heads in Transformers](./011-attention-heads-in-transformers.md) | How parallel attention heads calculate token relationships, combine outputs, and use KV caches during generation. |
| **012** | [Generation Loop](./012-generation-loop.md) | The inference-runtime loop that repeatedly asks the model for one next token until a stop condition is met. |
| **013** | [Numeric Attention Example With Three Tokens](./013-attention-numeric-example-three-tokens.md) | A concrete three-token, five-dimension worked example showing every Q, K, V, score, attention, and output table. |
| **014** | [Causal Attention Mask](./014-causal-attention-mask.md) | Explains how causal masking blocks future-token attention during training and generation. |
| **015** | [Transformer Block After Attention](./015-transformer-block-after-attention.md) | Shows what happens after attention inside a transformer block: residuals, normalization, and MLP. |
| **016** | [Output Layer, Logits, and Next Token](./016-output-layer-logits-and-next-token.md) | Explains how the final hidden vector becomes logits and a next-token choice. |
| **017** | [Sampler, Temperature, Top-k, and Top-p](./017-sampler-temperature-top-k-top-p.md) | Explains how the runtime chooses one token from model logits or probabilities. |
| **018** | [KV Cache During Generation](./018-kv-cache-during-generation.md) | Explains how cached key/value vectors speed up one-token-at-a-time generation. |
| **020** | [History Roadmap: How We Reached Modern LLMs](./020-history-road-to-llms.md) | Timeline of inventions, problems, solutions, key papers, and side branches leading to modern LLM systems. |
| **100** | [LLM Single Facts](./100-single-facts.md) | Short independent facts about LLM internals. |

## Numbering rule

Each page number must be unique.

When referring to a page in chat, use the page number, for example: 004
