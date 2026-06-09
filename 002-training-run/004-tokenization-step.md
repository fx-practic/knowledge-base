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
| Batch of sequences | `B x T` token IDs |

`T` means sequence length. `B` means batch size.

## Related page

See [Tokens](../004-tokens.md).
