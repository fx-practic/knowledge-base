Path: 002-training-run/006-embedding-lookup-step.md

# 006. Embedding Lookup Step

Token IDs are converted into vectors.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Token IDs, shape `T`. | Look up one row in the embedding table for each token ID. | Token vector matrix, shape `T x D`. |

With a mini-batch:

```text
B x T
-> B x T x D
```

Meaning:

```text
B training samples
each sample has T token IDs
each token ID becomes a D-dimensional vector
```

## What this step means

The embedding table is a trained table, not a neural network by itself. It maps
each token ID to one learned coordinate vector.

## Related page

See [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md).
