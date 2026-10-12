Path: 003-inference-run/017-append-token-step.md

# 017. Append Token Step

The selected token becomes part of the context.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Previous token IDs and one selected token ID. | Append selected token ID to the sequence. | Longer token sequence. |

Example:

```text
[A, B, C] + [D]
-> [A, B, C, D]
```

## What this step means

The model predicts one token at a time. A long answer is produced because the
runtime repeatedly appends the selected token and asks for another next token.

## Related page

See [Generation Loop Step](./019-generation-loop-step.md).
