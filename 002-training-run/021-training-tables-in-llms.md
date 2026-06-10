Path: 002-training-run/021-training-tables-in-llms.md

# 021. Trainable Tensors In LLMs

An LLM is a chain of mathematical transformations.

It contains many **trainable tensors**:

- embedding table;
- attention projection matrices;
- MLP / feed-forward weights;
- normalization weights;
- output layer weights.

The older phrase **training tables** is less precise. The better technical term is **trainable tensors** or **parameter tensors**.

## 1. Embedding table

The embedding table directly does this:

```text
token_id -> vector
```

The numbers inside are **not probabilities**. They are learned coordinates/features.

The embedding table is a trainable tensor, but it is not a full model by itself.

## 2. Training is done together

The embedding table is not trained separately.

During training:

```text
input tokens
-> embedding table
-> transformer blocks
-> logits
-> loss
-> backpropagation
-> optimizer update
```

All trainable tensors are updated together through the same computation graph.

## 3. Loss, gradients, and updates

The model compares its logits with the target token IDs and calculates **loss**.

Backpropagation calculates how changing each parameter would affect the loss locally. This local derivative is the **gradient**.

A gradient does not give the final correct value of a parameter. It gives local direction and relative strength for the next update.

Simple gradient descent:

```text
new_weight = weight - learning_rate * gradient
```

So training is many small optimisation steps, not one jump to perfect weights.

## 4. Why parameters do not all change equally

A weight usually works with some input or activation:

```text
output = w1*x1 + w2*x2 + w3*x3
```

Each weight gets a different gradient because it is connected to different inputs and different places in the computation graph.

Parameters do not move synchronously.

## 5. Chain rule

A parameter affects loss through many intermediate operations:

```text
parameter
-> layer computation
-> next layer
-> logits
-> loss
```

The chain rule allows backpropagation to calculate the local effect of each parameter on the final loss.

## 6. Meaning of trainable tensors

The tensor names come from their role in the architecture, not from guaranteed human-readable meaning.

| Part | Role |
| --- | --- |
| Embedding table | Converts token ID into starting vector. |
| Q/K/V projection matrices | Create query, key, and value vectors. |
| Attention output projection | Mixes head outputs back into model dimension. |
| MLP weights | Transform internal token features. |
| Normalization weights | Help stabilize numeric scale. |
| Output layer | Converts final hidden vectors into logits. |

The learned meaning is usually distributed across many tensors, not stored cleanly in one place.

## Core conclusion

An LLM is not a set of human-designed semantic tables.

It is a large trainable computation graph.

The architecture defines where each trainable tensor is used. Training adjusts the numbers so that the whole chain predicts the next token better.