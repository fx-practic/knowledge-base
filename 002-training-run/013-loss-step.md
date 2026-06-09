Path: 002-training-run/013-loss-step.md

# 013. Loss Step

Loss measures how wrong the model was.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Logits, shape `B x T x VocabSize`, and target token IDs, shape `B x T`. | Compare predicted scores with correct next tokens. | One loss value, or loss values averaged over the batch. |

## What this step means

Training needs a number that says whether the current weights produced good or
bad predictions. The loss is that number.

The loss is not the same thing as the gradients. Loss is the error measure.
Gradients are local directions calculated from that error measure.

## Related page

See [Training Tables in LLMs](./021-training-tables-in-llms.md).
