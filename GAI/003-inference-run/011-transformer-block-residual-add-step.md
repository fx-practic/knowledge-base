Path: 003-inference-run/011-transformer-block-residual-add-step.md

# 011. Transformer Block: Residual Add Step

After attention, the transformer block adds the block input back to the
attention output.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Original block input and attention output. | Residual add operation. | Matrix, shape `T x D`. |

## What this step means

This step keeps the same table shape. Residual add operations keep earlier
information available through deep stacks.

Approximate sense: the model keeps the previous token representation and adds
the attention result on top of it, instead of replacing the representation
completely.

## Related pages

See [Residual Add](../core/015-residual-add.md) and
[Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
