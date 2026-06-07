Path: 001-general-index.md

# 001. LLM General Index

This repository is the knowledge base root.

## Folder Maps

| Folder | Index | Meaning |
| --- | --- | --- |
| `core/` | [Core Index](./core/001-core-index.md) | Reusable transformer mechanisms shared by training and inference. |
| `training-run/` | [Training Run Index](./training-run/001-training-run-index.md) | Operations specific to learning/updating model weights. |
| `inference-run/` | [Inference Run Index](./inference-run/001-inference-run-index.md) | Operations specific to producing text from a trained model. |

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **002** | [Glossary](./002-glossary.md) | Terms and two-way links. |
| **003** | [Data Collection For LLM Training](./003-data-collection.md) | How internet, code, and other data are collected, filtered, and used for training corpora. |
| **004** | [Tokens](./004-tokens.md) | Tokens, tokenizers, vocabulary, tokenization algorithms. |
| **005** | [What An LLM Model Is Built From And What Files Store It](./005-model-files.md) | What physical parts a saved LLM contains and which files store them. |
| **006** | [ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md) | Maps classic ML terms such as sample, label, target, and prediction to LLM training terms. |
| **007** | [Training](./training-run/007-training.md) | Forward calculation flow and model training notes. |
| **008** | [Training Tables in LLMs](./training-run/008-training-tables-in-llms.md) | How trainable matrices, gradients, and backpropagation work together during LLM training. |
| **010** | [Embedding Output Shape and Positional Information](./core/010-embedding-output-shape-and-positional-information.md) | Why embedding lookup produces a T x D matrix and how models add token-position information. |
| **011** | [Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md) | How parallel attention heads calculate token relationships, combine outputs, and use KV caches during generation. |
| **012** | [Generation Loop](./inference-run/012-generation-loop.md) | The inference-runtime loop that repeatedly asks the model for one next token until a stop condition is met. |
| **013** | [Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md) | A concrete three-token, five-dimension worked example showing every Q, K, V, score, attention, and output table. |
| **014** | [Causal Attention Mask](./core/014-causal-attention-mask.md) | Explains how causal masking blocks future-token attention during training and generation. |
| **015** | [Residual Add](./core/015-residual-add.md) | Explains why transformer blocks add the original input back after attention or MLP transformations. |
| **016** | [Transformer Block After Attention](./core/016-transformer-block-after-attention.md) | Shows what happens after attention inside a transformer block: residuals, normalization, and MLP. |
| **017** | [Output Layer, Logits, and Next Token](./core/017-output-layer-logits-and-next-token.md) | Explains how the final hidden vector becomes logits and a next-token choice. |
| **018** | [Sampler, Temperature, Top-k, and Top-p](./inference-run/018-sampler-temperature-top-k-top-p.md) | Explains how the runtime chooses one token from model logits or probabilities. |
| **019** | [KV Cache During Generation](./inference-run/019-kv-cache-during-generation.md) | Explains how cached key/value vectors speed up one-token-at-a-time generation. |
| **020** | [History Roadmap: How We Reached Modern LLMs](./020-history-road-to-llms.md) | Timeline of inventions, problems, solutions, key papers, and side branches leading to modern LLM systems. |
| **021** | [Transformer Notation and Sizes](./core/021-transformer-notation-and-sizes.md) | Brief reference for transformer symbols, operations, parameter matrices, and realistic size values. |
| **022** | [Inference Runtime](./inference-run/022-inference-runtime.md) | Explains the software layer, such as Ollama or llama.cpp, that loads model weights and runs generation. |
| **100** | [LLM Single Facts](./100-single-facts.md) | Short independent facts about LLM internals. |

## Numbering rule

Each page number must be unique.

When referring to a page in chat, use the page number, for example: 004
