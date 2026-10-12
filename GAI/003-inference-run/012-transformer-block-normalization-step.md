Path: 003-inference-run/012-transformer-block-normalization-step.md

# 012. Transformer Block: Normalization Step

Normalization is a sub-operation inside each transformer block.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Matrix, shape `T x D`. | Normalize values inside token rows. | Matrix, shape `T x D`. |

## What this step means

Normalization keeps numeric scale more stable before the next sub-operation.
Different transformer families place normalization before or after attention
and MLP, but the beginner shape idea stays the same:

```text
T x D -> T x D
```

Approximate sense: normalization keeps vector values in a usable numeric range,
so later operations receive a more controlled signal.

## Related page

See [Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
