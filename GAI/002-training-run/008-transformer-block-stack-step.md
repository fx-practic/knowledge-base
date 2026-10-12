Path: 002-training-run/008-transformer-block-stack-step.md

# 008. Transformer Block Stack Step

The position-aware vectors pass through many transformer blocks.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Matrix `X`, shape `T x D`. | Apply transformer block 1, then block 2, and so on. | Updated matrix, shape `T x D`. |

With a mini-batch:

```text
B x T x D
-> B x T x D
```

## What this step means

Each block transforms the same table shape. The rows still correspond to token
positions, but the values become more context-aware after every block.

Approximate sense: the stack repeatedly refines token vectors. Early blocks may
capture simpler local patterns; later blocks can combine information into more
abstract context-aware features.

Steps `009`-`013` are not separate stages after the transformer stack. They are
zoom-in pages for operations inside each transformer block.

| Inside each transformer block | Page |
| --- | --- |
| Attention | [009. Transformer Block: Attention Step](./009-transformer-block-attention-step.md) |
| Causal mask | [010. Transformer Block: Causal Mask Step](./010-transformer-block-causal-mask-step.md) |
| Residual add | [011. Transformer Block: Residual Add Step](./011-transformer-block-residual-add-step.md) |
| Normalization | [012. Transformer Block: Normalization Step](./012-transformer-block-normalization-step.md) |
| MLP | [013. Transformer Block: MLP Step](./013-transformer-block-mlp-step.md) |

## Related page

See [Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
