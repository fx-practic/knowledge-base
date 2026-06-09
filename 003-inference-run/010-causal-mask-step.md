Path: 003-inference-run/010-causal-mask-step.md

# 010. Causal Mask Step

The causal mask prevents looking into future positions.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Attention score table, shape `T x T`. | Block future-token scores before softmax. | Masked score table, shape `T x T`. |

## What this step means

For the initial prompt pass, each prompt token position may only use allowed
previous/current positions. During one-token generation with KV cache, the new
token has no future generated tokens to inspect.

## Related page

See [Causal Attention Mask](../core/014-causal-attention-mask.md).
