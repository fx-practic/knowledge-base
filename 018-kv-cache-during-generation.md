Path: 018-kv-cache-during-generation.md

# 018. KV Cache During Generation

## Core idea

During generation, an LLM produces one token at a time.

Attention needs key and value vectors from previous tokens. Recomputing all
previous keys and values for every new token would be wasteful.

The **KV cache** stores previous key and value vectors.

```text
KV cache = stored K and V rows from earlier token positions
```

## Without KV cache

For every new token, the model would repeatedly process the whole prefix:

```text
token 1
token 1, token 2
token 1, token 2, token 3
...
```

This repeats work for old tokens.

## With KV cache

The model stores previous keys and values.

For a new token, it calculates mostly the new token's projections and reuses
cached earlier projections.

| Object | Shape for one head | Meaning |
| --- | --- | --- |
| Past keys | `T_past x d_head` | Stored key rows for previous token positions. |
| Past values | `T_past x d_head` | Stored value rows for previous token positions. |
| New query | `1 x d_head` | Query row for the current new token. |
| New key | `1 x d_head` | Key row to append to cache. |
| New value | `1 x d_head` | Value row to append to cache. |

## Attention with cache

For the new token:

| Operation | Shape |
| --- | --- |
| `Q_new` | `1 x d_head` |
| `K_cache^T` | `d_head x T_total` |
| `scores = Q_new * K_cache^T` | `1 x T_total` |
| `A = softmax(scores)` | `1 x T_total` |
| `V_cache` | `T_total x d_head` |
| `output = A * V_cache` | `1 x d_head` |

where:

```text
T_total = previous tokens + current token
```

The output is one updated vector for the current token.

## What grows?

The model weights do not grow.

The cache grows with generated context length.

| Thing | Grows during generation? |
| --- | --- |
| Model weights | No |
| Current text context | Yes |
| KV cache | Yes |
| Vocabulary size | No |

## Why cache is useful

| Benefit | Explanation |
| --- | --- |
| Less repeated computation | Previous key/value vectors are reused. |
| Faster generation | Each new token can attend to stored context. |
| Better practical serving | Runtime avoids recalculating old projections repeatedly. |

## Important limitation

The KV cache can become large for long contexts.

That is why modern models and serving systems care about:

```text
context length
number of layers
number of heads
d_head
KV-cache precision
MHA / MQA / GQA design
```

## Core conclusion

The KV cache is not new knowledge and not new training.

It is a runtime memory of previous key and value vectors:

```text
previous token positions
-> stored K and V rows
-> reused when generating the next token
```

It helps the generation loop produce tokens faster.
