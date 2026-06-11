Path: 003-inference-run/013-transformer-block-mlp-step.md

# 013. Transformer Block: MLP Step

The MLP is a sub-operation inside each transformer block.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Matrix, shape `T x D`. | MLP / feed-forward transformation applied to each token row. | Matrix, shape `T x D`. |
| Pre-MLP matrix and MLP output. | Residual add operation. | Block output, shape `T x D`. |

## What this step means

Attention mixes information between token positions. The MLP transforms each
token row internally after attention.

Approximate sense: after attention brings in context from other tokens, the MLP
reshapes the features inside each token row. It is per-token internal
transformation, while attention is token-to-token communication.

## Related page

See [Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
