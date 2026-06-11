Path: 003-inference-run/014-output-logits-step.md

# 014. Output Logits Step

The model converts the final hidden vector into raw scores for possible next tokens.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Final hidden matrix, shape `T x D`. | Take the last token row and apply output projection. | Logits vector, shape `1 x VocabSize`. |

## What this step means

During inference, the runtime usually needs the logits for the last position.

Logits are raw scores, not probabilities.

Approximate sense: this step converts the final internal vector into one score
for every possible next token. The sampler later chooses from these scores.

## Related page

See [Output Layer, Logits, and Next Token](../core/017-output-layer-logits-and-next-token.md).
