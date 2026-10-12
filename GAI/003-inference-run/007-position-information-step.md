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

Approximate sense: this step tells the model where each token is located in the
prompt, so the same words in a different order can lead to a different next
token prediction.

## Related page

See [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md).
