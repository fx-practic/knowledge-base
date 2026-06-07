Path: core/015-residual-add.md

# 015. Residual Add

## What X means

In transformer explanations, `X` usually means the current input matrix at a
particular stage.

```text
X shape: T x D
```

where:

```text
T = number of token positions
D = model dimension
```

Each row of `X` is one token vector.

## Basic idea

A natural expectation is:

```text
input
-> transformation
-> transformed result
-> next layer
```

Transformers usually do this instead:

```text
input
-> transformation
-> transformed result
-> add original input back: Residual add operation
-> next stage
```

So attention produces a result, but the transformer block does not use only
that result. It uses:

```text
input + attention_result
```

This is the **Residual add operation**.

## Why not just use attention result?

Attention is better understood as an **update**, not a full replacement.

The attention output says:

```text
Here is what this token learned from other tokens.
```

The original input still contains useful information:

```text
Here is what this token already was.
```

So the model keeps both:

```text
new_token_vector = old_token_vector + information_from_attention
```

That addition is the **Residual add operation**.

## Shape

Both matrices must have the same shape:

```text
X                shape: T x D
attention_output shape: T x D
```

Then:

```text
X + attention_output = T x D
```

The addition is cell by cell:

```text
same row, same column
```

It does not concatenate matrices.

## Small numeric example

Before attention:

```text
dog vector = [1.0, 2.0, 3.0]
```

Attention output:

```text
update from context = [0.1, -0.4, 0.2]
```

Residual add operation:

```text
[1.0, 2.0, 3.0] + [0.1, -0.4, 0.2]
= [1.1, 1.6, 3.2]
```

Without residual add, the next stage would receive only:

```text
[0.1, -0.4, 0.2]
```

That could throw away too much of the original token representation.

## More accurate attention flow

Attention is not exactly:

```text
input * certain attention
```

More accurately:

```text
input X
-> create Q, K, V
-> create attention weights A
-> attention_output = A * V
-> output projection
-> add original input back: Residual add operation
```

So the flow is:

```text
X
-> attention transformation
-> attention_output
-> X + attention_output
```

The original `X` skips around the attention transformation and joins back
afterward. This is why residual connections are also called **skip
connections**.

## Why it helps

Residual add lets a layer learn:

```text
what should be changed
```

instead of forcing it to:

```text
recreate the whole token vector from scratch
```

If a layer has nothing useful to add, it can produce something close to zero:

```text
X + 0 = X
```

So the layer can safely do little or nothing.

This helps deep networks train because information and gradients can pass
through many layers more easily.

## Transformer block pattern

A simplified transformer block uses residual add twice:

| Stage | Operation | Shape |
| --- | --- | --- |
| Attention | `attention_output = Attention(X)` | `T x D` |
| Residual add operation | `X + attention_output` | `T x D` |
| MLP | `mlp_output = MLP(...)` | `T x D` |
| Residual add operation | previous matrix `+ mlp_output` | `T x D` |

The exact order of normalization can differ between model families, but the
residual idea stays the same.

## Core conclusion

The simple layer idea is:

```text
layer output = transformation(input)
```

The residual transformer idea is:

```text
layer output = input + transformation(input)
```

The transformation still matters, but it is treated as an addition/update to
the original representation, not a complete replacement.
