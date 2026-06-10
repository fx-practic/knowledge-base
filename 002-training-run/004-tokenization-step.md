Path: 002-training-run/004-tokenization-step.md

# 004. Tokenization Step

Tokenization converts text into token IDs.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Text corpus. | Tokenizer splits text into tokens and maps each token to an integer ID. | Sequence of token IDs. |

Example:

```text
"cat sat"
-> ["cat", " sat"]
-> [1247, 9231]
```

## Shape idea

| Object | Shape |
| --- | --- |
| One training sequence | `T` token IDs |
| One mini-batch of sequences | `B x T` token IDs |

`T` means sequence length. `B` means mini-batch size.

## Mini-batch idea

A **mini-batch** is a small group of training samples processed together.
People often shorten this to **batch**, but **mini-batch** is more precise.

Example:

```text
B = 32 training samples
T = 100 tokens per sample

B x T = 32 x 100 token IDs
```

The model does not process a trillion samples at once. The training corpus is
split into many mini-batches:

```text
mini-batch 1 -> forward -> loss -> backpropagation -> optimizer update
mini-batch 2 -> forward -> loss -> backpropagation -> optimizer update
mini-batch 3 -> forward -> loss -> backpropagation -> optimizer update
...
```

So the optimizer update usually happens after a mini-batch, not after every
single training sample and not after the whole corpus.

## Related glossary terms

- [Mini-batch](../002-glossary.md#mini-batch)
- [Training Sample](../002-glossary.md#training-sample)

## Related page

See [Tokens](../004-tokens.md).
