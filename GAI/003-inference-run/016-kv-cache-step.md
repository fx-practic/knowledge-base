Path: 003-inference-run/016-kv-cache-step.md

# 016. KV Cache Step

## Core idea

During generation, an LLM produces one token at a time.

Attention needs key and value vectors, meaning K and V from attention
operations, computed from tokens already in the current context. Recomputing
those key and value vectors for every new token would be wasteful.

The **KV cache** stores K/V attention vectors computed from prompt tokens and
from generated tokens. It does not store the token IDs themselves.

```text
KV cache = stored K and V rows computed from context token positions
```

Why store K and V?

```text
new token's Q
+ cached context K
+ cached context V
= attention output for the new token
```

The cached K rows are used for matching/scoring. The cached V rows are used for
mixing information into the new token's updated vector.

## Without KV cache

For every new token, the model would repeatedly process the whole prefix:

```text
token 1
token 1, token 2
token 1, token 2, token 3
...
```

This repeats internal K/V calculation work for tokens already in the context.

## With KV cache

The runtime stores already-computed key and value vectors.

For a new token, it calculates mostly the new token's attention projections and
reuses cached K/V projections from prompt tokens and earlier generated tokens.

Important distinction:

| Object | What it is |
| --- | --- |
| Token IDs | The text represented as vocabulary IDs. |
| K/V vectors | Internal attention vectors computed from token representations. |
| KV cache | Runtime storage for those already-computed K/V vectors. |

| Object | Shape for one head | Meaning |
| --- | --- | --- |
| Past keys | `T_past x d_head` | Stored key rows already computed from context positions. |
| Past values | `T_past x d_head` | Stored value rows already computed from context positions. |
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
T_total = cached context positions + current token position
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
| Less repeated computation | Already-computed key/value vectors are reused. |
| Faster generation | Each new token can attend to cached K/V vectors from the context. |
| Better practical serving | Runtime avoids recalculating old K/V projections repeatedly. |

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

It is a runtime memory of already-computed key and value vectors:

```text
prompt tokens and generated tokens
-> computed K and V rows
-> stored in KV cache
-> reused when generating the next token
```

It helps the generation loop produce tokens faster.
