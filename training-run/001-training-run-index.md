Path: training-run/001-training-run-index.md

# 001. Training Run Index

Training is the run where model parameters are changed.

## Main flow

```text
training data
-> tokenization
-> forward pass through core model operations
-> logits
-> loss
-> backpropagation
-> optimizer update
-> changed weights
```

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **007** | [Training](./007-training.md) | Forward calculation flow and joint parameter updates. |
| **008** | [Training Tables in LLMs](./008-training-tables-in-llms.md) | How trainable matrices, gradients, and backpropagation work together. |

## Shared core operations

| Step | Core page |
| --- | --- |
| Embeddings and positions | [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md) |
| Attention | [Attention Heads in Transformers](../core/011-attention-heads-in-transformers.md) |
| Causal mask | [Causal Attention Mask](../core/014-causal-attention-mask.md) |
| Residual add | [Residual Add](../core/015-residual-add.md) |
| Transformer block | [Transformer Block After Attention](../core/016-transformer-block-after-attention.md) |
| Logits | [Output Layer, Logits, and Next Token](../core/017-output-layer-logits-and-next-token.md) |

## Training-specific difference

Training does not stop after choosing a next token. It compares predictions
with targets, calculates loss, runs backpropagation, and updates weights.
