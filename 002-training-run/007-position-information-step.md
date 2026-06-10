Path: 002-training-run/007-position-information-step.md

# 007. Position Information Step

The model adds information about token order.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Token vectors, shape `T x D`. | Add or inject position information. | Position-aware token vectors, shape `T x D`. |

With a mini-batch:

```text
B x T x D
-> B x T x D
```

## What this step means

Without position information, the model would have a weak view of order.

```text
dog bites man
man bites dog
```

The tokens are similar, but the meaning is different because the positions are
different.

## Related page

See [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md).
