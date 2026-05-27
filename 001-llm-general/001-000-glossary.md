`Path: 001-llm-general\\001-000-glossary.md`

# 001-000. Glossary

## Token

A unit of text that an LLM reads and predicts.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-100. RAG](./001-100-rag.md)

---

## Tokenizer

The component that converts raw text into tokens and token IDs.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

---

## Vocabulary

The full set of tokens known by a tokenizer.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

---

## Vocabulary Size

The number of possible token IDs in a tokenizer vocabulary.

Used in:

- [001-002. Tokens](./001-002-tokens.md)

---

## Token ID

The numeric identifier assigned to a token.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

---

## Embedding

A learned vector representation of a token ID.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

---

## Context Size

The maximum number of tokens a model can process in one context window.

Used in:

- [001-002. Tokens](./001-002-tokens.md)

---

## BPE

Byte Pair Encoding, a common tokenizer training method.

Used in:

- [001-002. Tokens](./001-002-tokens.md)

---

## SentencePiece

A tokenizer framework often used for LLM tokenization.

Used in:

- [001-002. Tokens](./001-002-tokens.md)

---

## Model Architecture

The structure of the model: number of layers, hidden size, attention heads, context length, vocabulary size, and other design parameters.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Config

A file or object that describes the model architecture and settings needed to rebuild the model structure.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Weights

Learned numeric parameters inside the model.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Tensor

An array of numbers used to store model parameters, such as vectors, matrices, or higher-dimensional blocks.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Checkpoint

A saved model state, usually containing trained weights and metadata needed to reload the model.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Transformer Layer

One repeated block inside a transformer model, usually containing attention, feed-forward networks, and normalization.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Attention

A mechanism that lets tokens interact and decide which previous tokens are important for the current computation.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Safetensors

A file format commonly used to store model tensors safely and efficiently.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## RAG

Retrieval-Augmented Generation.

Used in:

- [001-100. RAG](./001-100-rag.md)

---

## Fine-Tuning

Additional training performed after the base model training.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-100. RAG](./001-100-rag.md)
