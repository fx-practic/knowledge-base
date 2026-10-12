Path: 003-inference-run/008-transformer-block-stack-step.md

# 008. Transformer Block Stack Step

The prompt vectors pass through transformer blocks.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Matrix `X`, shape `T x D`. | Run the stack of transformer blocks using fixed trained weights. | Final hidden matrix, shape `T x D`. |

## What this step means

During inference, the weights do not change. The runtime only applies the fixed
trained operations to calculate the next-token distribution.

Approximate sense: the stack repeatedly refines token vectors using the trained
weights. Each block makes the current representation more useful for predicting
the next token.

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
