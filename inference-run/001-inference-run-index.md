Path: inference-run/001-inference-run-index.md

# 001. Inference Run Index

Inference is the run where a trained model is used to produce tokens.

## Main flow

```text
user prompt
-> tokenizer
-> inference runtime
-> forward pass through core model operations
-> logits
-> sampler
-> chosen token
-> generation loop repeats
```

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **012** | [Generation Loop](./012-generation-loop.md) | Runtime loop that repeatedly asks the model for one next token. |
| **018** | [Sampler, Temperature, Top-k, and Top-p](./018-sampler-temperature-top-k-top-p.md) | How the runtime chooses one token from logits or probabilities. |
| **019** | [KV Cache During Generation](./019-kv-cache-during-generation.md) | How cached key/value vectors speed up generation. |
| **022** | [Inference Runtime](./022-inference-runtime.md) | Software layer that loads model weights and runs generation. |

## Shared core operations

| Step | Core page |
| --- | --- |
| Embeddings and positions | [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md) |
| Attention | [Attention Heads in Transformers](../core/011-attention-heads-in-transformers.md) |
| Causal mask | [Causal Attention Mask](../core/014-causal-attention-mask.md) |
| Residual add | [Residual Add](../core/015-residual-add.md) |
| Transformer block | [Transformer Block After Attention](../core/016-transformer-block-after-attention.md) |
| Logits | [Output Layer, Logits, and Next Token](../core/017-output-layer-logits-and-next-token.md) |

## Inference-specific difference

Inference does not update weights. It uses the trained weights, chooses output
tokens, manages runtime memory such as KV cache, and stops according to runtime
rules.
