Path: 003-inference-run/006-embedding-lookup-step.md

# 006. Embedding Lookup Step

Prompt token IDs are converted into vectors.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Prompt token IDs, shape `T`. | Look up one embedding-table row per token ID. | Token vector matrix, shape `T x D`. |

## What this step means

There is still one row per token position. The model has not produced an answer
yet. It has only converted token IDs into internal numeric vectors.

Approximate sense: this step gives each prompt token its starting numeric
representation. It is like replacing token IDs with learned feature vectors
that later transformer blocks can transform.

## Related page

See [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md).
