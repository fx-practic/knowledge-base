Path: 007-training.md

# 007. Training

## Core distinction

Forward calculation is sequential, but training updates the model parameters
jointly.

## Forward calculation flow

During one forward pass, data flows through the model in order:

```text
token IDs
-> embedding table
-> positional information
-> transformer layer 1
-> transformer layer 2
-> ...
-> output prediction layer
-> next-token prediction
```

Each stage receives the output of the previous stage.

## Training update flow

During training, the model compares its prediction with the target next token
and calculates loss. Backpropagation then calculates gradients for the
trainable parameters that contributed to that loss.

```text
prediction
-> loss
-> backpropagation
-> gradients
-> optimizer update
```

The embedding table, attention matrices, MLP weights, and output layer are not
trained as completely separate systems. They are updated as parts of one
connected computation graph.

## Short summary

| Question | Answer |
| --- | --- |
| Does data move through the model sequentially? | Yes. |
| Are model parts trained one by one as isolated tables? | No. |
| Are trainable parts updated together from the same loss? | Yes. |
