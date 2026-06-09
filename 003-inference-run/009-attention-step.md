Path: 003-inference-run/009-attention-step.md

# 009. Attention Step

Attention mixes information between allowed token positions.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| `X`, shape `T x D`. | Apply `W_Q`, `W_K`, and `W_V` to each token row. | `Q`, `K`, and `V`, each shape `T x D_head` per head. |
| `Q` and `K`. | Calculate attention scores. | Score table, shape `T x T` per head. |
| Scores and `V`. | Weighted mixing of value vectors. | Attention output, shape `T x D_head` per head. |
| All heads. | Concatenate and project back. | Attention output, shape `T x D`. |

## What this step means

After attention, the result is still a token-position table. Each row is now
more context-aware because it has read information from other allowed rows.

## Related pages

See [Attention Heads in Transformers](../core/011-attention-heads-in-transformers.md)
and [Numeric Attention Example With Three Tokens](../core/013-attention-numeric-example-three-tokens.md).
