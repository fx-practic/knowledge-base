`Path: 001-llm-general\001-000-glossary.md`

# 001-000. Glossary

## Attention

A mechanism that lets tokens interact and decide which previous tokens are important for the current computation.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## BPE

Byte Pair Encoding, a common tokenizer training method.

Used in:

- [001-002. Tokens](./001-002-tokens.md)

---

## Checkpoint

A saved model state, usually containing trained weights and metadata needed to reload the model.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Config

A file or object that describes the model architecture and settings needed to rebuild the model structure.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Context Size

The maximum number of tokens a model can process in one context window.

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

## Feed-Forward Network

A neural network block inside a transformer layer that transforms token vectors after attention.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Fine-Tuning

Additional training performed after the base model training.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-100. RAG](./001-100-rag.md)

---

## Generation Config

Optional settings that define default text generation behavior, such as temperature, sampling, or maximum output length.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Input Sequence

A sequence of token IDs provided to the model as one training input.

Used in:

- [001-004. ML Training vs LLM Training Terms](./001-004-ml-training-vs-llm-training-terms.md)

---

## Label

The correct answer used for training. In LLM training, this is usually the next token ID or shifted target sequence.

Used in:

- [001-004. ML Training vs LLM Training Terms](./001-004-ml-training-vs-llm-training-terms.md)

---

## Model Architecture

The structure of the model: number of layers, hidden size, attention heads, context length, vocabulary size, and other design parameters.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Model Folder

A directory containing the files needed to load a saved model, usually including config, tokenizer files, and trained weights.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Output Layer

The final model component that converts the last internal vector into scores for possible next tokens.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Positional Encoding

A system that represents token order so the model can distinguish different word positions in the context.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Prediction

The model output before comparison with the target or label.

Used in:

- [001-004. ML Training vs LLM Training Terms](./001-004-ml-training-vs-llm-training-terms.md)

---

## RAG

Retrieval-Augmented Generation.

Used in:

- [001-100. RAG](./001-100-rag.md)

---

## Safetensors

A file format commonly used to store model tensors safely and efficiently.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## SentencePiece

A tokenizer framework often used for LLM tokenization.

Used in:

- [001-002. Tokens](./001-002-tokens.md)

---

## Target

The expected correct output for a training sample. In LLM training, it is usually the next token ID or a shifted sequence of next token IDs.

Used in:

- [001-004. ML Training vs LLM Training Terms](./001-004-ml-training-vs-llm-training-terms.md)

---

## Tensor

An array of numbers used to store model parameters, such as vectors, matrices, or higher-dimensional blocks.

Used in:

- [001-003. Model Files](./001-003-model-files.md)

---

## Token

A unit of text that an LLM reads and predicts.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-004. ML Training vs LLM Training Terms](./001-004-ml-training-vs-llm-training-terms.md)
- [001-100. RAG](./001-100-rag.md)

---

## Token ID

The numeric identifier assigned to a token.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

---

## Tokenizer

The component that converts raw text into tokens and token IDs.

Used in:

- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

---

## Training Sample

An input example used during training. In LLM training, it is usually a token sequence with a corresponding next-token target sequence.

Used in:

- [001-004. ML Training vs LLM Training Terms](./001-004-ml-training-vs-llm-training-terms.md)

---

## Transformer Layer

One repeated block inside a transformer model, usually containing attention, feed-forward networks, and normalization.

Used in:

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
- [001-003. Model Files](./001-003-model-files.md)

---

## Weights

Learned numeric parameters inside the model.

Used in:

- [001-003. Model Files](./001-003-model-files.md)
