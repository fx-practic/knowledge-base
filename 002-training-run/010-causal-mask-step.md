Path: 002-training-run/010-causal-mask-step.md

# 010. Causal Mask Step

The causal mask blocks attention to future tokens.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Attention score table, shape `T x T`. | Replace future-token scores with blocked values before softmax. | Masked score table, shape `T x T`. |

## What this step means

During next-token training, token position 3 may look at positions 1, 2, and 3,
but not position 4. This prevents the model from cheating by seeing the answer.

## Related page

See [Causal Attention Mask](../core/014-causal-attention-mask.md).
