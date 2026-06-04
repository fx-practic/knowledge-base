Path: 016-output-layer-logits-and-next-token.md

# 016. Output Layer, Logits, and Next Token

## Core idea

After all transformer blocks, the model still has a matrix:

```text
final hidden states: T x D
```

To predict the next token, the model usually uses the last token row and an
output layer.

## Final hidden matrix

| Object | Shape | Meaning |
| --- | --- | --- |
| Final hidden states | `T x D` | One final vector row per token position. |
| Last token row | `1 x D` | The vector used to predict the next token in decoder-only generation. |

Example:

```text
T = 3
D = 4096

final hidden states = 3 x 4096
last row            = 1 x 4096
```

## Output layer

The output layer maps the final vector into one score per vocabulary token.

Let:

```text
Vocab = vocabulary size
```

Then:

| Operation | Shape |
| --- | --- |
| `last_hidden` | `1 x D` |
| `W_out` | `D x Vocab` |
| `logits = last_hidden * W_out` | `1 x Vocab` |

Example:

```text
D = 4096
Vocab = 100000

1 x 4096 times 4096 x 100000 = 1 x 100000
```

The result has one score for every possible next token.

## Logits

Logits are raw scores before probability conversion.

| Token candidate | Logit |
| --- | ---: |
| `the` | 5.2 |
| `dog` | 2.1 |
| `.` | 1.7 |
| `banana` | -3.4 |

Higher logit means the model currently considers that token more likely or
more suitable.

Logits are not probabilities yet.

## Probabilities

Softmax converts logits into probabilities:

```text
probabilities = softmax(logits)
```

Shape stays:

```text
1 x Vocab
```

Example:

| Token candidate | Probability |
| --- | ---: |
| `the` | 0.61 |
| `dog` | 0.15 |
| `.` | 0.10 |
| `banana` | 0.001 |

## Choosing the next token

The runtime chooses one token from the output distribution.

| Method | Meaning |
| --- | --- |
| Greedy | Choose the highest-probability token. |
| Sampling | Randomly choose using probabilities. |
| Temperature / top-k / top-p | Adjust or restrict sampling behavior. |

The chosen token is appended to the context.

## One-step generation summary

| Stage | Shape / object |
| --- | --- |
| Prompt tokens | `T` token IDs |
| Final hidden states | `T x D` |
| Last hidden row | `1 x D` |
| Output layer weights | `D x Vocab` |
| Logits | `1 x Vocab` |
| Probabilities | `1 x Vocab` |
| Chosen next token | `1 token ID` |

## Core conclusion

The output layer converts:

```text
one final token vector
```

into:

```text
one score for every vocabulary token
```

Then the sampler or decoder chooses the next token.
