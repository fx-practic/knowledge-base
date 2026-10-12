Path: 002-training-run/006-embedding-lookup-and-mini-batches-step.md

# 006. Embedding Lookup and Mini-Batches Step

Token IDs are converted into vectors.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Token IDs, shape `T`. | Look up one row in the embedding table for each token ID. | Token vector matrix, shape `T x D`. |

With a mini-batch:

```text
B x T
-> B x T x D
```

Meaning:

```text
B training samples
each sample has T token IDs
each token ID becomes a D-dimensional vector
```

Here:

```text
B = number of training samples in this mini-batch
T = number of token positions in each sample
D = vector dimension
```

Modern LLM training often describes mini-batch size in **tokens**:

```text
tokens in mini-batch = B * T
```

Example:

```text
B = 2048 samples
T = 2048 tokens per sample
B * T = 4,194,304 tokens
```

Public reports mention large global mini-batches, such as about 2M, 4M, or 16M
tokens. This does not mean one GPU holds all tokens at once. The work is split
across many GPUs and sometimes accumulated from smaller micro-batches.

You can imagine `B x T x D` as a 3D block:

```text
B tables, each table is T x D
```

Later transformer weights are often 2D matrices. The same 2D weight matrix is
applied to every token vector inside this `B x T x D` block.

## When weights change

Usually they change after one mini-batch has completed:

```text
mini-batch
-> forward pass
-> loss
-> backpropagation
-> optimizer update
-> weights changed
```

So weights usually change after a mini-batch pass, not after one single
training sample and not after the whole corpus.

## Why not one training sample?

Training with one sample is possible, but the gradient is usually very noisy
and wastes GPU parallelism.

With a mini-batch, training averages the signal:

```text
sample 1 gradient
+ sample 2 gradient
+ sample 3 gradient
+ ...
-> average gradient
-> optimizer update
```

This is calculable because backpropagation works through the averaged loss.

"More stable" means the update direction is less dependent on one unusual
training sample. Loss and gradients usually jump less from one update to the
next, because the gradient is averaged across many samples. So the optimizer
gets a smoother signal: less random deviation, less drastic change in gradient
direction, and less noisy weight updates.

Mini-batches are also much faster in practice because GPUs can process many
similar vector/matrix operations in parallel. Very large mini-batches can hurt
training dynamics, so the size is tuned.

## What this step means

The embedding table is a trained table, not a neural network by itself. It maps
each token ID to one learned coordinate vector.

Approximate sense: this step gives each token its starting numeric
representation. It is like replacing token IDs with learned feature vectors
that later transformer blocks can transform.

## Related page

See [Embedding Output Shape and Positional Information](../core/010-embedding-output-shape-and-positional-information.md).

## Sources

- [Pythia paper](https://arxiv.org/pdf/2304.01373)
- [OLMo paper](https://kyleclo.com/assets/pdf/olmo-accelerating-the-science-of-language-models.pdf)
- [Llama 3 training systems paper](https://dl.acm.org/doi/10.1145/3695053.3731410)
