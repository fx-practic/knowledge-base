Path: 002-training-run/005-target-token-shift-step.md

# 005. Target Token Shift Step

Training needs both input tokens and target tokens.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| One token sequence. | Shift the sequence by one position. | Input IDs and target IDs. |

Example:

```text
tokens:  [A, B, C, D]
input:   [A, B, C]
target:  [B, C, D]
```

## What this step means

At each position, the model is trained to predict the next token.

| Position | Model sees | Correct target |
| --- | --- | --- |
| 1 | `A` | `B` |
| 2 | `A B` | `C` |
| 3 | `A B C` | `D` |

The target is not a human label like "cat" or "dog". In LLM training, the
target is usually the next token from the same text.
