Path: 002-training-run/013-loss-step.md

# 013. Loss Step

## Purpose

Loss converts many prediction errors into one training signal.

## Input form

| Object | Shape / form | Meaning |
| --- | --- | --- |
| Logits | `B x T x VocabSize` | Raw vocabulary scores for each token position. |
| Target token IDs | `B x T` | Correct next-token IDs created by the target shift step. |

Here, `B` means mini-batch size. The model is not comparing predictions for the
whole training corpus at once.

## Operation

Usually, the training code applies cross-entropy loss.

```text
logits + target token IDs -> loss
```

Cross-entropy does not simply subtract two numbers. It measures how strongly the model scored the correct target token compared with other vocabulary tokens.

## Output form

| Output | Shape / form |
| --- | --- |
| Loss | One scalar value, or position losses averaged over the mini-batch. |

## What changed physically

Many logits are compressed into one error measure used for training.

## What did not change

The model weights are not changed by this step alone. Weight changes happen later in the optimizer update step.

The loss is not the same thing as the gradients. Loss is the error measure.
Gradients are local directions calculated from that error measure.

## Related glossary terms

- [Cross-Entropy Loss](../002-glossary.md#cross-entropy-loss)
- [Logits](../002-glossary.md#logits)
- [Mini-batch](../002-glossary.md#mini-batch)
- [Target](../002-glossary.md#target)

## Related page

See [Trainable Tensors In LLMs](./021-training-tables-in-llms.md).
