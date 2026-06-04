Path: 013-attention-numeric-example-three-tokens.md

# 013. Numeric Attention Example With Three Tokens

## Goal

This page shows attention as physical tables, not only as formulas.

We use:

```text
Input text: dog bites man
T = 3 tokens
D = 5 embedding/model dimensions
one attention head
d_head = 5
```

The numbers are small educational numbers, not real weights from a trained
production model.

## Shape map

| Object | Shape | What it is |
| --- | --- | --- |
| Token IDs | `3` | Three token positions. |
| Embedding rows | `3 x 5` | One embedding vector per token. |
| Position rows | `3 x 5` | One position vector per token position. |
| `X` | `3 x 5` | Input matrix after adding position information. It contains one token vector row per token. |
| `W_Q` | `5 x 5` | Fixed learned query projection. |
| `W_K` | `5 x 5` | Fixed learned key projection. |
| `W_V` | `5 x 5` | Fixed learned value projection. |
| `Q` | `3 x 5` | Query matrix. It contains one query vector row per token. |
| `K` | `3 x 5` | Key matrix. It contains one key vector row per token. |
| `V` | `3 x 5` | Value matrix. It contains one value vector row per token. |
| `Q * K^T` | `3 x 3` | Every token compared with every token. |
| Attention weights `A` | `3 x 3` | How strongly each token reads from each token. |
| Attention output | `3 x 5` | One updated vector per token. |

## Important mental model

At every major step in this example, the model still keeps **one row per input
token position** until the attention-score table is created.

For this input:

```text
dog bites man
```

there are:

```text
3 token positions
```

So tables such as `X`, `Q`, `K`, `V`, and the final attention output all have
3 rows:

```text
one row for dog
one row for bites
one row for man
```

Attention does not remove token rows. It changes the numbers inside the rows.

## Step inventory: input form, output form, and number of objects

| Step | Input object(s) | Operation | Objects created | Output form |
| --- | --- | --- | --- | --- |
| Token lookup | 3 token IDs | Read rows from embedding table | 1 matrix: `E` | `E shape: 3 x 5` |
| Add position | `E` and `P` | Add row by row | 1 matrix: `X` | `X shape: 3 x 5` |
| Query projection | `X` and `W_Q` | `X * W_Q` | 1 matrix: `Q` | `Q shape: 3 x 5` |
| Key projection | `X` and `W_K` | `X * W_K` | 1 matrix: `K` | `K shape: 3 x 5` |
| Value projection | `X` and `W_V` | `X * W_V` | 1 matrix: `V` | `V shape: 3 x 5` |
| Q/K/V projection group | `X`, `W_Q`, `W_K`, `W_V` | Run the three projections | 3 matrices: `Q`, `K`, `V` | three separate `3 x 5` matrices |
| Transpose keys | `K` | Turn rows into columns | 1 matrix: `K^T` | `K^T shape: 5 x 3` |
| Compare tokens | `Q` and `K^T` | `Q * K^T` | 1 matrix: `scores` | `scores shape: 3 x 3` |
| Scale scores | `scores` | divide by `sqrt(5)` | 1 matrix: `scaled_scores` | `scaled_scores shape: 3 x 3` |
| Apply causal mask | `scaled_scores` and mask | block future positions | 1 matrix: `masked_scores` | `masked_scores shape: 3 x 3` |
| Softmax | `masked_scores` | convert scores to weights | 1 matrix: `A_causal` | `A_causal shape: 3 x 3` |
| Mix values | `A_causal` and `V` | `A_causal * V` | 1 matrix: `O_causal` | `O_causal shape: 3 x 5` |

Short version:

```text
`X` is the input matrix at this stage.
The projection operations `X * W_Q`, `X * W_K`, and `X * W_V` create three
new matrices: Q, K, V.
Multiplying Q by K^T creates the 3 x 3 attention-score table.
Softmax converts scores into the attention-weight table.
Multiplying the attention-weight table by V creates the final 3 x 5 attention
output.
```

The final attention output is still a table with:

```text
height = number of token positions
width  = number of dimensions
```

In this example:

```text
O_causal = 3 x 5
```

It still has a row for `dog`, a row for `bites`, and a row for `man`, but the
numbers in those rows have been transformed by attention.

## 1. Tokens

| Position | Token | Example token ID |
| --- | --- | --- |
| 1 | `dog` | `17` |
| 2 | `bites` | `81` |
| 3 | `man` | `42` |

## 2. Embedding table rows

The full embedding table would contain many vocabulary rows. Here we show only
the three rows used by this input.

| Token ID | Token | d1 | d2 | d3 | d4 | d5 |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| `17` | `dog` | 0.19 | -0.10 | 0.38 | 0.00 | 0.29 |
| `81` | `bites` | -0.30 | 0.48 | 0.09 | 0.18 | -0.20 |
| `42` | `man` | 0.57 | 0.09 | -0.40 | 0.29 | -0.02 |

Shape:

```text
E = 3 x 5
```

## 3. Position vectors

| Position | p1 | p2 | p3 | p4 | p5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| pos 1 | 0.01 | 0.00 | 0.02 | 0.00 | 0.01 |
| pos 2 | 0.00 | 0.02 | 0.01 | 0.02 | 0.00 |
| pos 3 | 0.03 | 0.01 | 0.00 | 0.01 | 0.02 |

Shape:

```text
P = 3 x 5
```

## 4. Input matrix after embedding plus position

Formula:

```text
X = E + P
```

| Token row | d1 | d2 | d3 | d4 | d5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| `dog` | 0.20 | -0.10 | 0.40 | 0.00 | 0.30 |
| `bites` | -0.30 | 0.50 | 0.10 | 0.20 | -0.20 |
| `man` | 0.60 | 0.10 | -0.40 | 0.30 | 0.00 |

Shape:

```text
X = 3 x 5
```

Each row is one token vector.

## 5. Query projection matrix W_Q

`W_Q` is fixed after training. It is applied to every token row of `X`.

| Input dim | q1 | q2 | q3 | q4 | q5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| d1 | 0.80 | -0.40 | 0.00 | 1.20 | 0.40 |
| d2 | 0.00 | 1.60 | -0.80 | 0.40 | -1.20 |
| d3 | -1.20 | 0.80 | 2.00 | -0.40 | 0.00 |
| d4 | 0.40 | 0.00 | -1.60 | 0.80 | 1.20 |
| d5 | 1.60 | -0.80 | 0.40 | 0.00 | 0.80 |

Shape:

```text
W_Q = 5 x 5
```

## 6. Key projection matrix W_K

`W_K` is also fixed after training and also applied to every token row.

| Input dim | k1 | k2 | k3 | k4 | k5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| d1 | 0.40 | 1.20 | -0.80 | 0.00 | 0.80 |
| d2 | -0.80 | 0.40 | 1.60 | -1.20 | 0.00 |
| d3 | 1.20 | -0.40 | 0.00 | 0.80 | -1.60 |
| d4 | 0.00 | 0.80 | 0.40 | 1.60 | -0.40 |
| d5 | -0.40 | 0.00 | 1.20 | -0.80 | 2.00 |

Shape:

```text
W_K = 5 x 5
```

## 7. Value projection matrix W_V

`W_V` is fixed after training and applied to every token row.

| Input dim | v1 | v2 | v3 | v4 | v5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| d1 | 0.30 | 0.00 | -0.10 | 0.20 | 0.10 |
| d2 | 0.10 | -0.30 | 0.20 | 0.00 | 0.40 |
| d3 | -0.20 | 0.50 | 0.00 | 0.10 | -0.10 |
| d4 | 0.40 | 0.10 | 0.30 | -0.20 | 0.00 |
| d5 | 0.00 | -0.10 | 0.40 | 0.30 | 0.20 |

Shape:

```text
W_V = 5 x 5
```

## 8. Produce Q, K, and V

Formulas:

```text
Q = X * W_Q
K = X * W_K
V = X * W_V
```

Important:

```text
Each token row is multiplied by the same W_Q, W_K, and W_V.
```

This step creates **three separate matrices**:

```text
Q matrix: 3 x 5
K matrix: 3 x 5
V matrix: 3 x 5
```

Each of those matrices still has one row per input token:

```text
row 1 -> dog
row 2 -> bites
row 3 -> man
```

So yes: after `X * W_Q`, there are as many query vectors as there are input
tokens. The same is true for `X * W_K` and `X * W_V`.

### Q table

| Token row | q1 | q2 | q3 | q4 | q5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| `dog` | 0.160 | -0.160 | 1.000 | 0.040 | 0.440 |
| `bites` | -0.600 | 1.160 | -0.600 | -0.040 | -0.640 |
| `man` | 1.080 | -0.400 | -1.360 | 1.160 | 0.480 |

Shape:

```text
Q = 3 x 5
```

### K table

| Token row | k1 | k2 | k3 | k4 | k5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| `dog` | 0.520 | 0.040 | 0.040 | 0.200 | 0.120 |
| `bites` | -0.320 | -0.040 | 0.880 | -0.040 | -0.880 |
| `man` | -0.320 | 1.160 | -0.200 | 0.040 | 1.000 |

Shape:

```text
K = 3 x 5
```

### V table

| Token row | v1 | v2 | v3 | v4 | v5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| `dog` | -0.030 | 0.200 | 0.080 | 0.170 | 0.000 |
| `bites` | 0.020 | -0.060 | 0.110 | -0.150 | 0.120 |
| `man` | 0.390 | -0.200 | 0.050 | 0.020 | 0.140 |

Shape:

```text
V = 3 x 5
```

## 9. Transpose K

To compare `Q` with `K`, the model uses `K^T`.

| Key dim | `dog` | `bites` | `man` |
| --- | ---: | ---: | ---: |
| k1 | 0.520 | -0.320 | -0.320 |
| k2 | 0.040 | -0.040 | 1.160 |
| k3 | 0.040 | 0.880 | -0.200 |
| k4 | 0.200 | -0.040 | 0.040 |
| k5 | 0.120 | -0.880 | 1.000 |

Shape:

```text
K^T = 5 x 3
```

## 10. Raw attention scores

Formula:

```text
scores = Q * K^T
```

Shape:

```text
3 x 5 times 5 x 3 = 3 x 3
```

| Query token reads keys | key: `dog` | key: `bites` | key: `man` |
| --- | ---: | ---: | ---: |
| query: `dog` | 0.178 | 0.446 | 0.005 |
| query: `bites` | -0.374 | 0.182 | 1.016 |
| query: `man` | 0.781 | -1.995 | -0.011 |

Shape:

```text
scores = 3 x 3
```

This is the first 3 by 3 "token looks at token" table.

## 11. Scaled scores

Formula:

```text
scaled_scores = scores / sqrt(5)
```

| Query token reads keys | key: `dog` | key: `bites` | key: `man` |
| --- | ---: | ---: | ---: |
| query: `dog` | 0.080 | 0.199 | 0.002 |
| query: `bites` | -0.167 | 0.081 | 0.454 |
| query: `man` | 0.349 | -0.892 | -0.005 |

Shape:

```text
scaled_scores = 3 x 3
```

## 12. Full attention weights without causal mask

This table is useful for learning because all 9 positions are visible.

Formula:

```text
A_full = softmax(scaled_scores)
```

| Query token reads values | value: `dog` | value: `bites` | value: `man` |
| --- | ---: | ---: | ---: |
| query: `dog` | 0.328 | 0.369 | 0.303 |
| query: `bites` | 0.241 | 0.309 | 0.449 |
| query: `man` | 0.502 | 0.145 | 0.353 |

Shape:

```text
A_full = 3 x 3
```

Each row sums to about `1`.

## 13. Full attention output

Formula:

```text
O_full = A_full * V
```

Shape:

```text
3 x 3 times 3 x 5 = 3 x 5
```

| Output row | o1 | o2 | o3 | o4 | o5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| `dog` updated | 0.116 | -0.017 | 0.082 | 0.006 | 0.087 |
| `bites` updated | 0.174 | -0.060 | 0.076 | 0.004 | 0.100 |
| `man` updated | 0.126 | 0.021 | 0.074 | 0.071 | 0.067 |

Shape:

```text
O_full = 3 x 5
```

## 14. Causal attention mask

In a decoder-only LLM, future tokens are blocked.

For `dog bites man`:

| Query token | Can read `dog` | Can read `bites` | Can read `man` |
| --- | --- | --- | --- |
| `dog` | yes | no | no |
| `bites` | yes | yes | no |
| `man` | yes | yes | yes |

The shape is still:

```text
mask = 3 x 3
```

## 15. Causal attention weights

After applying the mask and softmax:

| Query token reads values | value: `dog` | value: `bites` | value: `man` |
| --- | ---: | ---: | ---: |
| query: `dog` | 1.000 | 0.000 | 0.000 |
| query: `bites` | 0.438 | 0.562 | 0.000 |
| query: `man` | 0.502 | 0.145 | 0.353 |

Shape:

```text
A_causal = 3 x 3
```

Now future information is removed.

## 16. Causal attention output

Formula:

```text
O_causal = A_causal * V
```

Shape:

```text
3 x 3 times 3 x 5 = 3 x 5
```

| Output row | o1 | o2 | o3 | o4 | o5 |
| --- | ---: | ---: | ---: | ---: | ---: |
| `dog` updated | -0.030 | 0.200 | 0.080 | 0.170 | 0.000 |
| `bites` updated | -0.002 | 0.054 | 0.097 | -0.010 | 0.067 |
| `man` updated | 0.126 | 0.021 | 0.074 | 0.071 | 0.067 |

Shape:

```text
O_causal = 3 x 5
```

This is the final result of this one attention head in the causal version.

Important:

```text
O_causal is not one vector for the whole phrase.
O_causal is still one table with one row per token position.
```

Row meaning:

```text
row 1 -> updated dog vector
row 2 -> updated bites vector
row 3 -> updated man vector
```

The rows are still connected to the original token positions, but the vectors
are no longer plain embedding vectors. They are contextualized vectors created
by mixing allowed value vectors according to attention weights.

## 17. What changed physically?

| Stage | Shape | Number of visible numbers | Meaning |
| --- | --- | ---: | --- |
| `X` | `3 x 5` | 15 | Three token vectors enter attention. |
| `Q` | `3 x 5` | 15 | Query matrix: each row is one token's "what am I looking for?" vector. |
| `K` | `3 x 5` | 15 | Key matrix: each row is one token's "how can I be matched?" vector. |
| `V` | `3 x 5` | 15 | Value matrix: each row is one token's "what information can be mixed from me?" vector. |
| `scores` | `3 x 3` | 9 | Every token is compared with every token. |
| `A_causal` | `3 x 3` | 9 | Each token gets weights for allowed source tokens. |
| `O_causal` | `3 x 5` | 15 | Each token becomes an updated vector mixed from allowed value vectors. |

## 18. The row-by-row meaning of final causal output

| Output row | What it means |
| --- | --- |
| `dog` updated | It can read only `dog`, so it stays equal to the `dog` value row. |
| `bites` updated | It is a mix of `dog` and `bites`; it cannot read future token `man`. |
| `man` updated | It is a mix of `dog`, `bites`, and `man`. |

## Core conclusion

The most important physical picture is:

```text
X: 3 token rows x 5 dimensions
-> same W_Q, W_K, W_V applied to each row
-> Q, K, V: three separate 3 x 5 tables
-> Q * K^T: one 3 x 3 token-to-token score table
-> softmax: one 3 x 3 attention-weight table
-> A * V: one final 3 x 5 attention-output table
```

No token row disappears. Attention changes the numbers in each token row by
mixing value vectors from other allowed token rows.
