Path: core/011-attention-heads-in-transformers.md

# 011. Attention Heads in Transformers

## Goal of this page

This page separates **shape transformations** from explanation.

The left column shows the minimum formula and matrix shapes.
The right column explains what is happening.

## Symbols

| Symbol | Meaning |
| --- | --- |
| `T` | Number of token positions in the current input. This can change. |
| `D` | Model dimension. This is fixed for the model. |
| `h` | Number of attention heads. |
| `d_head` | Dimension used by one head. Usually `D = h * d_head`. |
| `X` | Current token representation matrix. Shape: `T x D`. |
| `W_Q`, `W_K`, `W_V` | Learned projection matrices for query, key, and value. |
| `W_O` | Learned output projection after all heads are combined. |

Example:

| Formula / shape | Explanation |
| --- | --- |
| `T = 5`<br>`D = 4096`<br>`X = 5 x 4096` | The input has 5 token positions. Each token row has 4096 numbers. |
| `h = 32`<br>`d_head = 128`<br>`32 * 128 = 4096` | The layer uses 32 attention heads. Each head works with 128 dimensions. |

## Main attention pipeline for one head

| Formula / shape | Explanation |
| --- | --- |
| `X shape: T x D` | This is the matrix after embedding and position processing. Each row is one token vector. |
| `W_Q shape: D x d_head`<br>`W_K shape: D x d_head`<br>`W_V shape: D x d_head` | These are fixed trained matrices for one attention head. Their size does not depend on `T`. |
| `Q = X * W_Q`<br>`T x D` times `D x d_head`<br>`Q shape: T x d_head` | The model creates one query vector for each token. **The same `W_Q` is applied to every input token row.** |
| `K = X * W_K`<br>`T x D` times `D x d_head`<br>`K shape: T x d_head` | The model creates one key vector for each token. **The same `W_K` is applied to every input token row.** |
| `V = X * W_V`<br>`T x D` times `D x d_head`<br>`V shape: T x d_head` | The model creates one value vector for each token. **The same `W_V` is applied to every input token row.** |
| `scores = Q * K^T`<br>`T x d_head` times `d_head x T`<br>`scores shape: T x T` | Every token query is compared with every token key. The result is a token-to-token score table. |
| `scores / sqrt(d_head)`<br>`shape: T x T` | Scaling keeps attention scores numerically stable before softmax. The shape does not change. |
| `masked_scores`<br>`shape: T x T` | In decoder-only LLMs, future token positions are blocked before softmax. The shape still does not change. |
| `A = softmax(masked_scores)`<br>`A shape: T x T` | `A` is the attention-weight matrix. Each row says how strongly one token position should read from allowed token positions. |
| `head = A * V`<br>`T x T` times `T x d_head`<br>`head shape: T x d_head` | The attention weights mix value vectors. Each output row becomes one token updated with information from other tokens. |

## Same pipeline with concrete numbers

Assume:

```text
T = 5
D = 4096
d_head = 128
```

| Formula / shape | Explanation |
| --- | --- |
| `X shape: 5 x 4096` | Five token rows enter attention. |
| `W_Q shape: 4096 x 128`<br>`W_K shape: 4096 x 128`<br>`W_V shape: 4096 x 128` | Fixed trained matrices for one head. |
| `Q shape: 5 x 128`<br>`K shape: 5 x 128`<br>`V shape: 5 x 128` | Each of the 5 tokens now has a query, key, and value vector for this head. |
| `Q * K^T`<br>`5 x 128` times `128 x 5`<br>`scores shape: 5 x 5` | The model compares every token with every token. |
| `A = softmax(scores)`<br>`A shape: 5 x 5` | The score table becomes attention weights. |
| `head = A * V`<br>`5 x 5` times `5 x 128`<br>`head shape: 5 x 128` | This one head returns one updated vector per token, with 128 dimensions per vector. |

## Fixed weights and variable input length

| Formula / shape | Explanation |
| --- | --- |
| `W_Q shape: D x d_head` | `W_Q` has fixed size after training. It does not grow when the input has more tokens. |
| `X shape: 3 x D`<br>`Q shape: 3 x d_head` | If the input has 3 tokens, the same `W_Q` is applied to 3 token rows. |
| `X shape: 1000 x D`<br>`Q shape: 1000 x d_head` | If the input has 1,000 tokens, the same `W_Q` is applied to 1,000 token rows. |
| `token_i x W_Q -> query_i` | Row-by-row view: each token vector is multiplied by the same learned matrix. |
| `token_i x W_K -> key_i` | The same rule applies to `W_K`. It is reused for every token row. |
| `token_i x W_V -> value_i` | The same rule applies to `W_V`. It is reused for every token row. |

Analogy:

| Formula / shape | Explanation |
| --- | --- |
| `price x 1.20` | The formula is fixed. |
| `3 prices -> apply formula 3 times` | The formula does not change. |
| `1000 prices -> apply formula 1000 times` | The list length changes, not the formula. |

Core rule:

```text
Weights have fixed size because each token vector has fixed size D.
Input length can vary because the same weights are applied to each token row.
```

## What the T x T attention matrix means

For a sequence:

```text
dog bites man
```

`T = 3`, so the attention-weight matrix has shape:

```text
A shape: 3 x 3
```

| Formula / shape | Explanation |
| --- | --- |
| `A[row dog, column dog]` | How much the `dog` row reads from the `dog` value vector. |
| `A[row dog, column bites]` | How much the `dog` row reads from the `bites` value vector. |
| `A[row dog, column man]` | How much the `dog` row reads from the `man` value vector. |
| `A[row bites, column dog]` | How much the `bites` row reads from the `dog` value vector. |
| `A[row man, column bites]` | How much the `man` row reads from the `bites` value vector. |

So:

```text
T x T = token positions reading from token positions
```

Each row is one token asking:

```text
Which earlier or allowed token positions should influence my new vector?
```

## Multi-head attention

One head returns:

```text
T x d_head
```

With `h` heads:

| Formula / shape | Explanation |
| --- | --- |
| `head_1 shape: T x d_head` | First parallel attention result. |
| `head_2 shape: T x d_head` | Second parallel attention result. |
| `...` | More heads run in parallel. |
| `head_h shape: T x d_head` | Last head result. |
| `Concat(head_1, ..., head_h)`<br>`shape: T x (h * d_head)` | The model joins the head outputs by columns. |
| If `D = h * d_head`:<br>`Concat shape: T x D` | In the common case, concatenation returns to model dimension `D`. |
| `output = Concat * W_O`<br>`T x D` times `D x D`<br>`output shape: T x D` | `W_O` mixes information across all heads and returns the shape needed for the rest of the Transformer layer. |

Concrete example:

| Formula / shape | Explanation |
| --- | --- |
| `D = 4096`<br>`h = 32`<br>`d_head = 128` | The model dimension is split across 32 heads. |
| `each head: T x 128` | Each head returns 128 dimensions per token. |
| `concat: T x 4096` | 32 head outputs are joined together. |
| `W_O: 4096 x 4096`<br>`output: T x 4096` | Final projection keeps the same model shape. |

## Causal mask

In decoder-only LLMs, a token cannot read future tokens.

For `T = 4`, allowed reading pattern:

| Formula / shape | Explanation |
| --- | --- |
| `token 1 -> token 1` | First token can read only itself. |
| `token 2 -> token 1, token 2` | Second token can read earlier tokens and itself. |
| `token 3 -> token 1, token 2, token 3` | Third token cannot read token 4. |
| `token 4 -> token 1, token 2, token 3, token 4` | Fourth token can read all previous positions and itself. |
| `mask shape: T x T` | The mask has the same shape as the attention-score matrix. |
| `masked_scores shape: T x T` | Future positions are blocked before softmax. |

The causal mask prevents the model from cheating during next-token training.

## KV cache during generation

During generation, the model produces new tokens one by one.

Without caching, it would repeatedly recalculate keys and values for old
tokens. Instead, implementations store previous keys and values.

| Formula / shape | Explanation |
| --- | --- |
| `past K cache shape: T_past x d_head` | Stored key vectors for previous token positions in one head. |
| `past V cache shape: T_past x d_head` | Stored value vectors for previous token positions in one head. |
| `new token X shape: 1 x D` | The model processes one newly generated token row. |
| `new Q shape: 1 x d_head` | The new token needs a query. |
| `new K shape: 1 x d_head`<br>`new V shape: 1 x d_head` | New key and value are added to the cache. |
| `Q_new * K_cache^T`<br>`1 x d_head` times `d_head x T_total`<br>`scores shape: 1 x T_total` | The new token compares itself with all cached previous positions plus itself. |
| `attention output shape: 1 x d_head` | The head returns one updated vector for the new token. |

The KV cache grows as the context grows.

## Modern head variants

| Variant / shape | Explanation |
| --- | --- |
| **MHA**<br>many query heads<br>many key/value heads | Multi-Head Attention. Each query head has its own key/value projections. |
| **MQA**<br>many query heads<br>one shared key/value head | Multi-Query Attention. Reduces KV-cache size during generation. |
| **GQA**<br>many query heads<br>several key/value groups | Grouped-Query Attention. A compromise between MHA and MQA. |

Example:

| Formula / shape | Explanation |
| --- | --- |
| `32 query heads`<br>`8 key/value heads` | Several query heads share one key/value group. This is grouped-query attention. |

## Common misconceptions

| Misconception | Correction |
| --- | --- |
| "A head is one neuron." | A head is a full attention computation over token positions. |
| "A head is one table." | A head uses learned matrices plus matrix multiplication, softmax, masking, and value mixing. |
| "Attention means one matrix called attention." | Attention is the operation: create `Q/K/V`, compare `Q` with `K`, then mix `V`. |
| "The model chooses only one head." | Heads usually run in parallel and their outputs are combined. |
| "Each head has a manually assigned meaning." | Head behavior emerges during training. It is not guaranteed to be cleanly human-readable. |

## Shape summary

| Formula / shape | Explanation |
| --- | --- |
| `X: T x D` | Input token representations. |
| `W_Q, W_K, W_V: D x d_head` | Fixed trained matrices for one head. Reused for every token row. |
| `Q, K, V: T x d_head` | One query, key, and value vector per token. |
| `Q * K^T: T x T` | Token-to-token score matrix. |
| `softmax(...): T x T` | Token-to-token attention weights. |
| `A * V: T x d_head` | One head output. |
| `Concat heads: T x D` | All heads joined together. |
| `Concat * W_O: T x D` | Final attention output for the layer. |

## Core conclusion

Attention starts with:

```text
X: T x D
```

and ends with:

```text
output: T x D
```

The shape is preserved, but the meaning of each row changes.

Before attention:

```text
each row mostly represents one token
```

After attention:

```text
each row represents one token updated by reading information from other
allowed token positions
```
