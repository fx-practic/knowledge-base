Path: 002-glossary.md

# 002. Glossary

This glossary defines the main terms used in this LLM knowledge base.

---

## Activation

A nonlinear function used inside a neural network layer. In transformer MLP blocks, activations or gates help transform each token row beyond simple linear multiplication.

---

## ALiBi

Attention with Linear Biases. A positional method that adds a distance-based bias to attention scores instead of adding position vectors to token embeddings.

---

## Attention

A mechanism that lets token positions read information from previous or otherwise allowed token positions.

---

## Attention Head

One parallel attention computation inside a transformer layer. A head creates query, key, and value vectors, calculates attention weights, and returns updated token vectors.

---

## Attention Projection Weights

Learned parameter matrices inside attention, such as `W_Q`, `W_K`, `W_V`, and `W_O`.

These are stored model parameters. They are not the temporary `T x T` attention-weight table created during one forward pass.

---

## Attention Weights

The temporary attention distribution produced during a forward pass, usually after softmax is applied to attention scores.

Typical shape:

```text
T x T
```

Do not confuse this with **attention projection weights**, which are learned parameter matrices stored in the model.

---

## Backpropagation

The backward calculation that applies the chain rule through the computation graph to calculate gradients for trainable parameters.

---

## Batch

A group of training samples processed together in one training step.

Common LLM shapes:

```text
token IDs:  B x T
embeddings: B x T x D
```

---

## BPE

Byte Pair Encoding, a common tokenizer training method.

---

## Causal Attention Mask

A left-to-right attention rule that lets each token position attend only to itself and earlier positions, not later positions.

---

## Checkpoint

A saved model state, usually containing trained weights and metadata needed to reload the model.

---

## Computation Graph

The known chain of operations used in a forward pass. Backpropagation uses it to calculate how loss depends on trainable parameters.

---

## Config

A file or object that describes the model architecture and settings needed to rebuild the model structure.

---

## Context Size

The maximum number of tokens a model can process in one context window.

Related terms:

- context window
- context length

---

## Cross-Entropy Loss

A common loss function for classification and next-token prediction. In LLM training, it compares vocabulary logits or probabilities with the correct target token ID.

---

## Embedding

A learned vector representation of a token ID.

---

## Feed-Forward Network

A neural network block inside a transformer layer that transforms token vectors after attention. It is also commonly called the transformer MLP.

---

## Fine-Tuning

Additional training performed after base model training.

---

## Forward Pass

One execution of the model from input token IDs or token vectors to output logits. During a forward pass, parameters are used but not changed.

---

## Generation Config

Optional settings that define default text generation behavior, such as temperature, sampling, or maximum output length.

---

## Generation Loop

The inference-runtime loop that repeatedly asks the model for the next token, appends that token to the context, and stops when a stop condition is met.

---

## GQA

Grouped-Query Attention. A modern attention variant where multiple query heads share a smaller number of key/value heads.

---

## Gradient

A local derivative of the loss with respect to a parameter. It indicates the local direction and relative strength for changing that parameter.

---

## Greedy Decoding

A decoding method that chooses the highest-probability next token at each generation step.

---

## Hidden Size

The width of the main token vector inside the model. In this knowledge base it is usually written as `D` or `d_model`.

---

## Hidden State

An internal token representation produced inside the model. A final hidden state is the token representation after the transformer block stack, before the output projection.

---

## Inference Runtime

The software system that runs a trained model after training. It handles model calls, generation loop, sampling, KV cache, stop rules, and returning output text.

Related terms:

- model runtime
- LLM runtime
- inference engine
- serving runtime
- inference server

---

## Input Sequence

A sequence of token IDs provided to the model as one training input.

---

## Key

In attention, a key vector represents how a token position can be matched by query vectors from other token positions.

---

## KV Cache

A runtime cache of previous key and value vectors used during generation so the model does not need to recalculate them for every new token.

---

## Label

The correct answer used for training. In LLM training, this is usually the next token ID or shifted target sequence.

---

## LayerNorm

Layer normalization. A normalization method that stabilizes numeric scale inside neural network activations.

---

## Learning Rate

A training hyperparameter that controls the size of optimizer updates. In simple gradient descent:

```text
change = -learning_rate * gradient
```

---

## Logits

Raw output scores before softmax. In LLM generation, logits are used to choose the next token.

---

## MHA

Multi-Head Attention. The standard attention design where several attention heads run in parallel and their outputs are combined.

---

## MLP

Multi-Layer Perceptron. In a transformer block, this is the feed-forward network that transforms each token row internally after attention.

---

## Model Architecture

The structure of the model: number of layers, hidden size, attention heads, context length, vocabulary size, and other design parameters.

---

## Model Dimension

The width of the main token vector inside the transformer. In this knowledge base, this is usually written as `D` or `d_model`.

---

## Model Folder

A directory containing the files needed to load a saved model, usually including config, tokenizer files, and trained weights.

---

## MQA

Multi-Query Attention. An attention variant with many query heads but shared key/value heads, often used to reduce KV-cache cost.

---

## Optimizer

The training algorithm that uses gradients, learning rate, and optimizer state to update trainable parameters.

---

## Mini-batch

A small group of training samples processed together before one loss,
backpropagation pass, and optimizer update.

People often shorten this to **batch**, but **mini-batch** is more precise.

Example:

```text
mini-batch size B = 32
sequence length T = 100

B x T = 32 training samples, each with 100 token IDs
```

The model does not process the whole training corpus at once. It also usually
does not update weights after every single training sample. Instead, training
uses many mini-batches:

```text
mini-batch 1 -> forward -> loss -> backpropagation -> optimizer update
mini-batch 2 -> forward -> loss -> backpropagation -> optimizer update
mini-batch 3 -> forward -> loss -> backpropagation -> optimizer update
```

Used in:

- [004. Tokenization Step](./002-training-run/004-tokenization-step.md)
- [006. Embedding Lookup and Mini-Batches Step](./002-training-run/006-embedding-lookup-and-mini-batches-step.md)
- [015. Loss Step](./002-training-run/015-loss-step.md)
- [017. Optimizer Update Step](./002-training-run/017-optimizer-update-step.md)

---

## Output Layer

The final model component that converts final hidden vectors into logits for possible next tokens.

---

## Parameter

One learned number inside the model. Parameters are stored inside trainable tensors such as matrices, vectors, or higher-dimensional arrays.

---

## Positional Encoding

A system that represents token order so the model can distinguish different word positions in the context.

---

## Prediction

The model output before comparison with the target or label. In LLM training this is usually vocabulary logits or probabilities for next-token prediction.

---

## Probability Distribution

A set of probabilities over possible outcomes that usually sums to 1. In LLM generation, it is often a distribution over possible next tokens.

---

## Projection Matrix

A learned matrix that transforms vectors from one representation space into another, such as from token vectors into query, key, or value vectors.

---

## Query

In attention, a query vector represents what a token position is looking for when it compares itself with key vectors.

---

## RAG

Retrieval-Augmented Generation.

---

## Residual Add

An operation that adds a layer's original input matrix back to the layer's transformation output, usually with matching shape `T x D`.

---

## RMSNorm

Root Mean Square Layer Normalization. A normalization method commonly used in modern LLMs to stabilize numeric scale.

---

## RoPE

Rotary Position Encoding. A positional method that injects position information by rotating parts of query and key vectors.

---

## Safetensors

A file format commonly used to store model tensors safely and efficiently.

---

## Sampler

The runtime component that chooses the next token from model logits or probabilities during generation.

---

## SentencePiece

A tokenizer framework often used for LLM tokenization.

---

## Softmax

A function that converts raw scores into a probability distribution. In attention it converts scores into attention weights; in output processing it converts logits into token probabilities.

---

## Target

The expected correct output for a training sample. In LLM training, it is usually the next token ID or a shifted sequence of next token IDs.

---

## Temperature

A sampling setting that changes how sharp or flat the next-token probability distribution is.

---

## Tensor

An array of numbers used to store model parameters or activations, such as vectors, matrices, or higher-dimensional blocks.

---

## Token

A unit of text that an LLM reads and predicts.

---

## Token ID

The numeric identifier assigned to a token.

---

## Tokenizer

The component that converts raw text into tokens and token IDs.

---

## Top-k

A sampling method that keeps only the `k` highest-scoring token candidates before sampling.

---

## Top-p

A sampling method that keeps the smallest set of token candidates whose cumulative probability reaches threshold `p`.

---

## Trainable Parameter

A parameter that can be changed by the optimizer during training.

---

## Trainable Tensor

A tensor containing trainable parameters. Examples include embedding tables, attention projection matrices, MLP matrices, normalization vectors, and output projection matrices.

---

## Training Sample

An input example used during training. In LLM training, it is usually a token sequence with a corresponding next-token target sequence.

---

## Transformer Block

One repeated processing block inside a transformer model, usually containing attention, feed-forward/MLP transformation, residual connections, and normalization.

---

## Transformer Layer

Another common name for a transformer block. It usually contains attention, feed-forward networks, residual connections, and normalization.

---

## Value

In attention, a value vector contains the information that will be mixed into updated token vectors according to attention weights.

---

## Vocabulary

The full set of tokens known by a tokenizer.

---

## Vocabulary Size

The number of possible token IDs in a tokenizer vocabulary.

---

## Weights

Learned numeric parameters inside the model.
