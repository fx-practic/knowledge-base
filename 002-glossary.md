Path: 002-glossary.md

# 002. Glossary

## Attention

A mechanism that lets tokens interact and decide which previous tokens are important for the current computation.

Used in:

- [005. Model Files](./005-model-files.md)
- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)

---

## Attention Head

One parallel attention computation inside a transformer layer. A head creates query, key, and value vectors, calculates attention weights, and returns updated token vectors.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)

---

## ALiBi

Attention with Linear Biases. A positional method that adds a distance-based bias to attention scores instead of adding position vectors to token embeddings.

Used in:

- [010. Embedding Output Shape and Positional Information](./core/010-embedding-output-shape-and-positional-information.md)

---

## BPE

Byte Pair Encoding, a common tokenizer training method.

Used in:

- [004. Tokens](./004-tokens.md)

---

## Checkpoint

A saved model state, usually containing trained weights and metadata needed to reload the model.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Config

A file or object that describes the model architecture and settings needed to rebuild the model structure.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Context Size

The maximum number of tokens a model can process in one context window.

Used in:

- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

---

## Causal Attention Mask

A left-to-right attention rule that lets each token position attend only to itself and earlier positions, not later positions.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)
- [014. Causal Attention Mask](./core/014-causal-attention-mask.md)

---

## Embedding

A learned vector representation of a token ID.

Used in:

- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

---

## Feed-Forward Network

A neural network block inside a transformer layer that transforms token vectors after attention.

Used in:

- [005. Model Files](./005-model-files.md)
- [016. Transformer Block After Attention](./core/016-transformer-block-after-attention.md)

---

## Fine-Tuning

Additional training performed after the base model training.

Used in:

- [004. Tokens](./004-tokens.md)

---

## Generation Config

Optional settings that define default text generation behavior, such as temperature, sampling, or maximum output length.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Generation Loop

The inference-runtime loop that repeatedly asks the model for the next token, appends that token to the context, and stops when a stop condition is met.

Used in:

- [012. Generation Loop](./inference-run/012-generation-loop.md)

---

## GQA

Grouped-Query Attention. A modern attention variant where multiple query heads share a smaller number of key/value heads.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)

---

## Inference Runtime

The software system that runs a trained model after training. It handles model calls, generation loop, sampling, KV cache, stop rules, and returning output text.

Related terms:

- model runtime
- LLM runtime
- serving runtime
- inference server

Used in:

- [012. Generation Loop](./inference-run/012-generation-loop.md)
- [018. Sampler, Temperature, Top-k, and Top-p](./inference-run/018-sampler-temperature-top-k-top-p.md)
- [019. KV Cache During Generation](./inference-run/019-kv-cache-during-generation.md)
- [022. Inference Runtime](./inference-run/022-inference-runtime.md)

---

## Input Sequence

A sequence of token IDs provided to the model as one training input.

Used in:

- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)

---

## Key

In attention, a key vector represents how a token position can be matched by query vectors from other token positions.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)

---

## KV Cache

A runtime cache of previous key and value vectors used during generation so the model does not need to recalculate them for every new token.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [012. Generation Loop](./inference-run/012-generation-loop.md)

---

## Label

The correct answer used for training. In LLM training, this is usually the next token ID or shifted target sequence.

Used in:

- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)

---

## Model Architecture

The structure of the model: number of layers, hidden size, attention heads, context length, vocabulary size, and other design parameters.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Model Folder

A directory containing the files needed to load a saved model, usually including config, tokenizer files, and trained weights.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Logits

Raw output scores before softmax. In LLM generation, logits are used to choose the next token.

Used in:

- [012. Generation Loop](./inference-run/012-generation-loop.md)
- [017. Output Layer, Logits, and Next Token](./core/017-output-layer-logits-and-next-token.md)
- [018. Sampler, Temperature, Top-k, and Top-p](./inference-run/018-sampler-temperature-top-k-top-p.md)

---

## MHA

Multi-Head Attention. The standard attention design where several attention heads run in parallel and their outputs are combined.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)

---

## MQA

Multi-Query Attention. An attention variant with many query heads but shared key/value heads, often used to reduce KV-cache cost.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)

---

## MLP

Multi-Layer Perceptron. In a transformer block, this is the feed-forward network that transforms each token row internally after attention.

Used in:

- [016. Transformer Block After Attention](./core/016-transformer-block-after-attention.md)

---

## Output Layer

The final model component that converts the last internal vector into scores for possible next tokens.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Positional Encoding

A system that represents token order so the model can distinguish different word positions in the context.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Prediction

The model output before comparison with the target or label.

Used in:

- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)

---

## Projection Matrix

A learned matrix that transforms vectors from one representation space into another, such as from token vectors into query, key, or value vectors.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)

---

## Query

In attention, a query vector represents what a token position is looking for when it compares itself with key vectors.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)

---

## RAG

Retrieval-Augmented Generation.

Used in:

- [100. LLM Single Facts](./100-single-facts.md)

---

## Residual Add

An operation that adds a layer's original input matrix back to the layer's transformation output, usually with matching shape `T x D`.

Used in:

- [016. Transformer Block After Attention](./core/016-transformer-block-after-attention.md)
- [015. Residual Add](./core/015-residual-add.md)

---

## RoPE

Rotary Position Encoding. A positional method that injects position information by rotating parts of query and key vectors.

Used in:

- [010. Embedding Output Shape and Positional Information](./core/010-embedding-output-shape-and-positional-information.md)

---

## Safetensors

A file format commonly used to store model tensors safely and efficiently.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Sampler

The runtime component that chooses the next token from model logits or probabilities during generation.

Used in:

- [012. Generation Loop](./inference-run/012-generation-loop.md)

---

## SentencePiece

A tokenizer framework often used for LLM tokenization.

Used in:

- [004. Tokens](./004-tokens.md)

---

## Target

The expected correct output for a training sample. In LLM training, it is usually the next token ID or a shifted sequence of next token IDs.

Used in:

- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)

---

## Tensor

An array of numbers used to store model parameters, such as vectors, matrices, or higher-dimensional blocks.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Token

A unit of text that an LLM reads and predicts.

Used in:

- [004. Tokens](./004-tokens.md)
- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)

---

## Token ID

The numeric identifier assigned to a token.

Used in:

- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

---

## Tokenizer

The component that converts raw text into tokens and token IDs.

Used in:

- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

---

## Training Sample

An input example used during training. In LLM training, it is usually a token sequence with a corresponding next-token target sequence.

Used in:

- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)

---

## Transformer Layer

One repeated block inside a transformer model, usually containing attention, feed-forward networks, and normalization.

Used in:

- [005. Model Files](./005-model-files.md)

---

## Value

In attention, a value vector contains the information that will be mixed into updated token vectors according to attention weights.

Used in:

- [011. Attention Heads in Transformers](./core/011-attention-heads-in-transformers.md)
- [013. Numeric Attention Example With Three Tokens](./core/013-attention-numeric-example-three-tokens.md)

---

## Vocabulary

The full set of tokens known by a tokenizer.

Used in:

- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

---

## Vocabulary Size

The number of possible token IDs in a tokenizer vocabulary.

Used in:

- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

---

## Weights

Learned numeric parameters inside the model.

Used in:

- [005. Model Files](./005-model-files.md)
