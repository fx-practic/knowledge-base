Path: 002-training-run/011-transformer-block-residual-add-step.md

# 011. Transformer Block: Residual Add Step

After attention, the transformer block adds the block input back to the
attention output.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Original block input `X` and attention output `A_out`. | Residual add operation: `X + A_out`. | Matrix, shape `T x D`. |

## What this step means

Residual add operations keep earlier information available and help gradients
move through deep stacks.

Approximate sense: the model keeps the original token representation and adds
the new transformation on top of it, instead of replacing everything at once.

## Related pages

See [Residual Add](../core/015-residual-add.md) and
[Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
