Path: 015-transformer-block-after-attention.md

# 015. Transformer Block After Attention

## Core idea

Attention is only one part of a transformer block.

After attention, the model still has:

```text
T x D
```

where:

```text
T = number of token positions
D = model dimension
```

The shape usually stays `T x D` through the whole block.

## Main block flow

| Stage | Input form | Operation | Output form | Meaning |
| --- | --- | --- | --- | --- |
| Block input | `X: T x D` | Current token vectors enter the block. | `T x D` | One row per token position. |
| Attention | `T x D` | Tokens read information from other allowed tokens. | `A_out: T x D` | Token rows become context-enriched. |
| Residual add | `X + A_out` | Add the original block input back. | `T x D` | Keeps earlier information and helps training. |
| Normalization | `Norm(...)` | Stabilize numeric scale inside each token row. | `T x D` | Values become easier for later layers to process. |
| MLP / feed-forward | `T x D` | Transform each token row internally. | `M_out: T x D` | No token-to-token mixing here in the simple view. |
| Residual add | previous `T x D + M_out` | Add the pre-MLP representation back. | `T x D` | Keeps information and improves gradient flow. |
| Normalization | `Norm(...)` | Stabilize again. | `T x D` | Final block output. |

Different model families place normalization before or after attention/MLP.
The important beginner idea is that the shape stays `T x D`.

## Attention versus MLP

| Part | What it mainly does | Shape |
| --- | --- | --- |
| Attention | Mixes information between token positions. | `T x D -> T x D` |
| MLP / feed-forward | Transforms each token row internally. | `T x D -> T x D` |

Short version:

```text
Attention = communication between tokens.
MLP       = internal transformation inside each token row.
```

## Residual connection

A residual connection adds an earlier matrix back to a later matrix:

```text
new_X = old_X + transformation_output
```

Shapes:

```text
old_X                 T x D
transformation_output  T x D
new_X                 T x D
```

This does not concatenate matrices. It adds matching cells:

```text
same row, same column
```

## Normalization

Normalization adjusts the numeric scale of token vectors.

| Input | Output | What changes |
| --- | --- | --- |
| `T x D` | `T x D` | Numbers are rescaled/stabilized. |

The number of rows and columns does not change.

Modern LLMs often use RMSNorm instead of classic LayerNorm, but the beginner
shape idea is the same:

```text
normalization keeps T x D
```

## MLP / feed-forward network

The MLP usually expands each token vector into a larger internal dimension and
then projects it back.

| Stage | Shape | Meaning |
| --- | --- | --- |
| Input to MLP | `T x D` | One row per token. |
| First linear projection | `T x D -> T x D_ff` | Expand each token row. |
| Activation / gating | `T x D_ff` | Nonlinear transformation. |
| Second linear projection | `T x D_ff -> T x D` | Return to model dimension. |

`D_ff` is often larger than `D`.

Example:

```text
D = 4096
D_ff = 11008

T x 4096
-> T x 11008
-> T x 4096
```

## One full block as a shape table

| Step | Shape |
| --- | --- |
| Input | `T x D` |
| Attention output | `T x D` |
| After first residual/norm | `T x D` |
| MLP hidden expansion | `T x D_ff` |
| MLP output | `T x D` |
| After second residual/norm | `T x D` |
| Output to next block | `T x D` |

## Core conclusion

After attention, the model does not immediately predict the next token.

It continues through the rest of the transformer block:

```text
attention
-> residual / normalization
-> MLP
-> residual / normalization
-> next transformer block
```

The matrix shape usually remains:

```text
T x D
```

but the token vectors become more processed at every block.
