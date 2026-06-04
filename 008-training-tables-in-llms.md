Path: 008-training-tables-in-llms.md

# 008. Training Tables in LLMs

An **LLM is a chain of mathematical transformations**.
It contains many trainable tables/matrices: **embedding table**, **attention matrices**, **MLP weights**, **output layer**, etc.

## 1. Embedding table

The **embedding table** directly does only this:

```text
token_id -> vector
```

The numbers inside are **not probabilities**.
They are learned coordinates/features.

## 2. Training is done together

The embedding table is **not trained separately**.

During training:

```text
input tokens
-> embedding table
-> transformer layers
-> output probabilities
-> loss
-> backpropagation
-> update weights
```

All trainable parts are updated together.

## 3. Loss, gradients, and updates

The model compares its prediction with the correct target token and calculates **loss**.

Backpropagation does **not** find the "guilty" parameter.
It calculates how changing each parameter slightly would affect the loss.

This is the **gradient**.

A gradient does **not** give the exact optimal value.
It only shows the **local direction and slope** in the current point.

The actual update is controlled by:

```text
new_weight = weight - learning_rate * gradient
```

The calculated gradients are not final answers for the weights.
Their sum does not need to equal the loss, and a gradient is not the full
weight change by itself. In simple gradient descent, each gradient provides:

1. a **sign**, which determines whether the weight should increase or decrease;
2. a **magnitude**, which indicates how strongly that weight should move
   relative to other weights.

For one specific weight, there are only two directions: increase or decrease.
The step size is not simply `+learning_rate` or `-learning_rate`. It depends on
the gradient:

```text
change = -learning_rate * gradient
```

For example:

```text
grad_a = -16
grad_b = -32
grad_c = +48
learning_rate = 0.01

a += 0.16
b += 0.32
c -= 0.48
```

Here, `a` increases a little, `b` increases more strongly, and `c` decreases
even more strongly. With simple gradient descent, a gradient magnitude of
`100` produces a step one hundred times larger than a gradient magnitude of
`1`, if the same learning rate is used.

In practice, optimizers such as Adam can rescale updates using additional
information from current and previous gradients. Therefore, gradient
magnitudes still matter, but the final update ratio is not always exactly the
same as the raw gradient ratio.

So training is a sequence of many small steps, not one jump to the perfect solution.

## 4. Why parameters do not all change equally

A weight usually works with some input/activation:

```text
output = w1*x1 + w2*x2 + w3*x3
```

So each weight gets a different gradient because each weight is connected to different inputs and different positions in the model.

Parameters do **not** move synchronously.

## 5. Chain rule

Chain rule is needed because one parameter affects loss through many intermediate computations:

```text
parameter
-> layer computation
-> next layer
-> logits
-> loss
```

Chain rule allows backpropagation to calculate how a parameter affects the final loss through the whole chain.

## 6. Meaning of tables

The table names come from their **role in the architecture**, not from guaranteed human-readable meaning.

| Part | Role |
| --- | --- |
| **Embedding table** | converts token ID into starting vector |
| **Q/K/V matrices** | create query/key/value vectors for attention |
| **Attention** | controls which tokens influence each other |
| **MLP layers** | transform internal features |
| **Output layer** | converts final vectors into token probabilities |

The learned meaning is usually **distributed across many matrices**, not stored cleanly in one place.

## Personal understanding note

My current personal understanding is this:

For training optimisation, the model uses the known architecture, known mathematical operations, current parameter values, activations, target output, and loss to estimate how each coefficient should be changed.

I do not yet fully understand the exact mathematical mechanism of how chain rule extracts the update direction and relative update strength for every coefficient. But the idea is that because the whole computation graph is known, and because we designed the operations ourselves, backpropagation can calculate useful local gradients for the parameters.

So chain rule is not "finding the correct final value" of each parameter.
It is trying to estimate the next small optimisation step for each parameter: increase, decrease, and roughly how strongly.

## Core conclusion

An LLM is not a set of human-designed semantic tables.
It is a large trainable computation graph.

The architecture defines **where each table is used**.
Training adjusts the numbers so that the whole chain predicts the next token better.
