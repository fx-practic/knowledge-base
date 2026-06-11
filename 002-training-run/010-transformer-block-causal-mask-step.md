Path: 002-training-run/010-transformer-block-causal-mask-step.md

# 010. Transformer Block: Causal Mask Step

The causal mask is used inside the transformer block's attention operation. It
blocks attention to future tokens.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Attention score table, shape `T x T`. | Replace future-token scores with blocked values before softmax. | Masked score table, shape `T x T`. |

## What this step means

During next-token training, token position 3 may look at positions 1, 2, and 3,
but not position 4. This prevents the model from cheating by seeing the answer.

Approximate sense: the mask enforces the rule "predict from the past, not from
the future." It keeps training aligned with generation, where future tokens do
not exist yet.

## Related page

See [Causal Attention Mask](../core/014-causal-attention-mask.md).
