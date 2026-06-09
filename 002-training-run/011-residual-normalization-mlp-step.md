Path: 002-training-run/011-residual-normalization-mlp-step.md

# 011. Residual, Normalization, and MLP Step

After attention, the transformer block continues transforming the token table.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Original block input `X` and attention output `A_out`. | Residual add operation: `X + A_out`. | Matrix, shape `T x D`. |
| Matrix, shape `T x D`. | Normalize values inside token rows. | Matrix, shape `T x D`. |
| Matrix, shape `T x D`. | MLP / feed-forward transformation. | Matrix, shape `T x D`. |
| Pre-MLP matrix and MLP output. | Residual add operation again. | Block output, shape `T x D`. |

## What this step means

Attention mixes information between token positions. The MLP then transforms
each token row internally. Residual add operations keep earlier information
available and help gradients move through deep stacks.

## Related pages

See [Residual Add](../core/015-residual-add.md) and
[Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
