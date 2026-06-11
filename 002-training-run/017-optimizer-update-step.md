Path: 002-training-run/017-optimizer-update-step.md

# 017. Optimizer Update Step

The optimizer changes model weights.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Current parameters, gradients, learning rate, and optimizer state. | Optimizer rule calculates parameter updates. | Updated parameters. |

Simplified example:

```text
new_weight = weight - learning_rate * gradient
```

## What this step means

Backpropagation calculates gradients. The optimizer decides how to use those
gradients to update the weights.

After this step, the model has changed. The next mini-batch will use the
new parameter values.

## Related page

See [Training Tables in LLMs](./021-training-tables-in-llms.md).
