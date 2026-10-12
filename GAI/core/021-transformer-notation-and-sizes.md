Path: core/021-transformer-notation-and-sizes.md

# 021. Transformer Notation and Sizes

## Core idea

This page separates:

```text
data matrices
parameter matrices
operations
size symbols
```

## Common symbols

| Symbol | Type | Meaning |
| --- | --- | --- |
| `T` | size number | Number of token positions in the current input. |
| `d_model` | size number | Width of the main token vector inside the model. |
| `h` | size number | Number of attention heads. |
| `d_k` | size number | Width of query/key vectors for one head. |
| `d_v` | size number | Width of value vectors for one head. |
| `Vocab` | size number | Number of possible token IDs. |
| `X` | data matrix | Current token representations. Shape: `T x d_model`. |
| `W_Q` | parameter matrix | Learned query projection. |
| `W_K` | parameter matrix | Learned key projection. |
| `W_V` | parameter matrix | Learned value projection. |
| `Q` | data matrix | Query vectors created from `X * W_Q`. |
| `K` | data matrix | Key vectors created from `X * W_K`. |
| `V` | data matrix | Value vectors created from `X * W_V`. |
| `A` | data matrix | Attention-weight matrix. Shape: `T x T`. |

## Operations

| Operation | Input | Output |
| --- | --- | --- |
| `Q = X * W_Q` | `T x d_model` times `d_model x d_k` | `T x d_k` |
| `K = X * W_K` | `T x d_model` times `d_model x d_k` | `T x d_k` |
| `V = X * W_V` | `T x d_model` times `d_model x d_v` | `T x d_v` |
| `scores = Q * K^T` | `T x d_k` times `d_k x T` | `T x T` |
| `A = softmax(scores)` | `T x T` | `T x T` |
| `output = A * V` | `T x T` times `T x d_v` | `T x d_v` |

## Realistic modern values

These are realistic examples, not universal rules.

| Symbol | Typical modern range | Example value |
| --- | ---: | ---: |
| `T` | `1` to `100k+` tokens, depending on context window | `4096` |
| `d_model` | about `2048` to `8192+` | `4096` |
| `h` | about `16` to `64+` heads | `32` |
| `d_k` | often `64`, `128`, or `256` | `128` |
| `d_v` | often same as `d_k` | `128` |
| `Vocab` | about `32k` to `200k+` tokens | `128k` |

Example one-head shapes:

```text
T = 4096
d_model = 4096
d_k = 128
d_v = 128

X:   4096 x 4096
W_Q: 4096 x 128
Q:   4096 x 128

K:   4096 x 128
V:   4096 x 128
A:   4096 x 4096
out: 4096 x 128
```

## Short conclusion

```text
W_Q, W_K, W_V = learned parameters
Q, K, V       = data matrices produced during calculation
d_model, d_k, d_v = size numbers
```

The letters are notation. The important thing is the role:

```text
fixed weights transform variable-length token rows into Q, K, V tables
```
