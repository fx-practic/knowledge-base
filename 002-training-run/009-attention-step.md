Path: 002-training-run/009-attention-step.md

# 009. Attention Step

Attention lets token positions read information from other allowed token
positions.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| `X`, shape `T x D`. | Apply learned `W_Q`, `W_K`, and `W_V` to every token row. | `Q`, `K`, and `V`, each shape `T x D_head` per head. |
| `Q` and `K`. | Calculate attention scores. | Score table, shape `T x T` per head. |
| Scores and `V`. | Weighted mixing of value vectors. | Attention output, shape `T x D_head` per head. |
| All heads. | Concatenate and project back. | Attention output, shape `T x D`. |

## What this step means

The attention step does not destroy the token rows. After attention, the model
still has one row per token position, but each row now contains information
mixed from other allowed positions.

## Related pages

See [Attention Heads in Transformers](../core/011-attention-heads-in-transformers.md)
and [Numeric Attention Example With Three Tokens](../core/013-attention-numeric-example-three-tokens.md).
