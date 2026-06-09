Path: 002-training-run/012-output-logits-step.md

# 012. Output Logits Step

The model converts final hidden vectors into scores for vocabulary tokens.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Final hidden matrix, shape `T x D`. | Output projection to vocabulary size. | Logits matrix, shape `T x VocabSize`. |

With a batch:

```text
B x T x D
-> B x T x VocabSize
```

## What this step means

Each row contains raw scores for possible next tokens at that position. These
raw scores are logits, not probabilities yet.

## Related page

See [Output Layer, Logits, and Next Token](../core/017-output-layer-logits-and-next-token.md).
