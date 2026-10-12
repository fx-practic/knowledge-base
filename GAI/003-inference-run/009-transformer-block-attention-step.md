Path: 003-inference-run/009-transformer-block-attention-step.md

# 009. Transformer Block: Attention Step

Attention is a sub-operation inside each transformer block. It mixes
information between allowed token positions.

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

Approximate sense: attention lets each token decide which other tokens to read
from in the current prompt/context.

Why multiple heads? Because one head may learn one kind of relationship, and
another head may learn another kind. For example:

| Head | Possible learned pattern |
| --- | --- |
| Head 1 | Nearby words. |
| Head 2 | Subject-verb relation. |
| Head 3 | Punctuation or structure. |
| Head 4 | Long-distance reference. |

These meanings are not guaranteed to be cleanly human-readable. They are memory
hooks for why multiple heads can be useful.

## Related pages

See [Attention Heads in Transformers](../core/011-attention-heads-in-transformers.md)
and [Numeric Attention Example With Three Tokens](../core/013-attention-numeric-example-three-tokens.md).
