Path: core/001-core-index.md

# 001. Core Index

Core pages explain reusable mechanisms used by both training and inference.

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **010** | [Embedding Output Shape and Positional Information](./010-embedding-output-shape-and-positional-information.md) | Why embedding lookup produces a T x D matrix and how models add token-position information. |
| **011** | [Attention Heads in Transformers](./011-attention-heads-in-transformers.md) | How parallel attention heads calculate token relationships and combine outputs. |
| **013** | [Numeric Attention Example With Three Tokens](./013-attention-numeric-example-three-tokens.md) | Concrete three-token, five-dimension attention walkthrough. |
| **014** | [Causal Attention Mask](./014-causal-attention-mask.md) | Left-to-right masking used in decoder-only transformers. |
| **015** | [Residual Add](./015-residual-add.md) | Why transformer blocks add original input back after transformations. |
| **016** | [Transformer Block After Attention](./016-transformer-block-after-attention.md) | Residuals, normalization, and MLP after attention. |
| **017** | [Output Layer, Logits, and Next Token](./017-output-layer-logits-and-next-token.md) | How hidden vectors become logits for possible next tokens. |
| **021** | [Transformer Notation and Sizes](./021-transformer-notation-and-sizes.md) | Symbols, operations, parameter matrices, and realistic size values. |

## Used by

| Run | How it uses core pages |
| --- | --- |
| [Training run](../training-run/001-training-run-index.md) | Uses core forward operations, then adds loss, backpropagation, and optimizer updates. |
| [Inference run](../inference-run/001-inference-run-index.md) | Uses core forward operations, then adds runtime, generation loop, sampler, and KV cache. |
