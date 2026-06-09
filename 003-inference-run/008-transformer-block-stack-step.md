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

## Related page

See [Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
