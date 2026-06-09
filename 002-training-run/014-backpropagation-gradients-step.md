Path: 002-training-run/014-backpropagation-gradients-step.md

# 014. Backpropagation and Gradients Step

Backpropagation calculates gradients for trainable parameters.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Loss and known computation graph. | Apply chain rule backward through the operations. | Gradient for each trainable parameter. |

## What this step means

A gradient is not the final correct value for a weight. It is not the exact
update by itself, and the sum of gradients is not the loss.

For each parameter, the gradient gives two practical pieces of information:

| Gradient property | Meaning |
| --- | --- |
| Sign | Whether increasing the parameter would increase or decrease loss locally. |
| Magnitude | Relative strength of the local effect compared with other parameters. |

## Related page

See [Training Tables in LLMs](./021-training-tables-in-llms.md).
