Path: 003-inference-run/012-output-logits-step.md

# 012. Output Logits Step

The model converts the final hidden vector into scores for possible next tokens.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Final hidden matrix, shape `T x D`. | Take the last token row and apply output projection. | Logits vector, shape `VocabSize`. |

## What this step means

During inference, the runtime usually needs the logits for the last position,
because the task is to choose the next token after the current prompt/context.

## Related page

See [Output Layer, Logits, and Next Token](../core/017-output-layer-logits-and-next-token.md).
