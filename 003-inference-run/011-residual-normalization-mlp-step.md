Path: 003-inference-run/011-residual-normalization-mlp-step.md

# 011. Residual, Normalization, and MLP Step

After attention, each transformer block continues processing the token table.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Original block input and attention output. | Residual add operation. | Matrix, shape `T x D`. |
| Matrix, shape `T x D`. | Normalization. | Matrix, shape `T x D`. |
| Matrix, shape `T x D`. | MLP / feed-forward transformation. | Matrix, shape `T x D`. |
| Pre-MLP matrix and MLP output. | Residual add operation. | Block output, shape `T x D`. |

## What this step means

This step keeps the same table shape. Attention handles token-to-token mixing.
The MLP transforms each token row internally.

## Related pages

See [Residual Add](../core/015-residual-add.md) and
[Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
