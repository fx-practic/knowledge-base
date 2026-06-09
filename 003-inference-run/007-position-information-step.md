Path: 003-inference-run/007-position-information-step.md

# 007. Position Information Step

The runtime/model adds token order information.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Token vectors, shape `T x D`. | Add or inject position information. | Position-aware token vectors, shape `T x D`. |

## What this step means

The same token can mean different things in different positions. Position
information lets the model distinguish order-sensitive text.

## Related page

See [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md).
