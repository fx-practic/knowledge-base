Path: 001-general-index.md

# 001. LLM General Index

This repository is the knowledge base root.

## Folder Maps

| Folder | Index | Meaning |
| --- | --- | --- |
| `core/` | [Core Index](./core/001-core-index.md) | Reusable transformer mechanisms shared by training and inference. |
| `002-training-run/` | [Training Run Index](./002-training-run/002-training-run-index.md) | Step-by-step run where model weights are updated. |
| `003-inference-run/` | [Inference Run Index](./003-inference-run/003-inference-run-index.md) | Step-by-step run where a trained model produces text. |
| `004-rag-run/` | [RAG Run Index](./004-rag-run/004-rag-run-index.md) | Step-by-step run where external knowledge is retrieved and added to model context. |

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **002** | [Glossary](./002-glossary.md) | Terms and two-way links. |
| **003** | [Data Collection For LLM Training](./003-data-collection.md) | How data is collected, filtered, and used for training corpora. |
| **004** | [Tokens](./004-tokens.md) | Tokens, tokenizers, vocabulary, tokenization algorithms. |
| **005** | [What An LLM Model Is Built From And What Files Store It](./005-model-files.md) | What physical parts a saved LLM contains and which files store them. |
| **006** | [ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md) | Maps classic ML terms such as sample, label, target, and prediction to LLM training terms. |
| **007** | [Bridge From Classic MLP Training To LLM Training](./007-mlp-reader-bridge.md) | Explains LLM training for a reader familiar with classic multi-layer perceptrons. |
| **010** | [Embedding Output Shape and Positional Information](./core/010-embedding-output-shape-and-positional-information.md) | Why embedding lookup produces a T x D matrix and how models add token-position information. |
| **011** | [Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md) | How parallel attention heads calculate token relationships, combine outputs, and use KV caches during generation. |
| **013** | [Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md) | A concrete three-token, five-dimension worked example showing every Q, K, V, score, attention, and output table. |
| **014** | [Causal Attention Mask](./core/014-causal-attention-mask.md) | Explains how causal masking blocks future-token attention during training and generation. |
| **015** | [Residual Add](./core/015-residual-add.md) | Explains why transformer blocks add the original input back after attention or MLP transformations. |
| **016** | [Transformer Block After Attention](./core/016-transformer-block-after-attention.md) | Shows what happens after attention inside a transformer block: residuals, normalization, and MLP. |
| **017** | [Output Layer, Logits, and Next Token](./core/017-output-layer-logits-and-next-token.md) | Explains how the final hidden vector becomes logits and a next-token choice. |
| **020** | [History Roadmap: How We Reached Modern LLMs](./020-history-road-to-llms.md) | Timeline of inventions, problems, solutions, key papers, and side branches leading to modern LLM systems. |
| **021** | [Transformer Notation and Sizes](./core/021-transformer-notation-and-sizes.md) | Brief reference for transformer symbols, operations, parameter matrices, and realistic size values. |
| **022** | [Questions And Answers](./022-questions-and-answers.md) | Short Q&A notes about LLMs, Codex, and practical model behavior. |
| **004-rag-run/004** | [RAG Run Index](./004-rag-run/004-rag-run-index.md) | Runtime retrieval pipeline for improving LLM answers with external context. |
| **100** | [LLM Single Facts](./100-single-facts.md) | Short independent facts about LLM internals. |

## Numbering rule

Page numbers are unique inside one folder. The folder indexes make the main learning order visible:

```text
core/001
002-training-run/002
003-inference-run/003
004-rag-run/004
```

When referring to a root page in chat, use the page number, for example: 004.
For folder pages, use the folder and number, for example:
`002-training-run/009` or `003-inference-run/015`.
