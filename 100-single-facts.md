Path: 100-single-facts.md

# 100. LLM Single Facts

Related pages:

- [001. LLM General Index](./001-general-index.md)
- [005. What An LLM Model Is Built From And What Files Store It](./005-model-files.md)

## Facts

| Number | Fact |
| --- | --- |
| F008 | The output of a transformer layer has size d for each token. |
| F009 | RAG means Retrieval-Augmented Generation: the system searches external documents and puts relevant passages into the prompt, so the model can answer using that context without permanently changing its weights. |
| F010 | [Embedding vector dimensions contain coordinates, not probabilities.](#f010-embedding-vector-coordinate-values) Their expected numerical scale depends on the model and any normalization applied. |
| F011 | [An embedding table is a trained lookup table, not a neural network by itself.](#f011-an-embedding-table-is-not-a-neural-network) |
| F012 | [Attention is an operation, not just a table.](#f012-attention-is-an-operation-not-just-a-table) It computes how strongly token positions should influence one another. |

## F010. Embedding Vector Coordinate Values

An embedding vector is a list of coordinates in a multidimensional space. Its
individual values are not probabilities, so they do not need to be between
`0` and `1`, and negative values are normal.

In many trained models, raw coordinate values are relatively small: values
within ranges such as approximately `-5` to `5`, or sometimes `-10` to `10`,
are plausible. However, there is no universal valid range. Some embedding
systems normalize the whole vector to unit length, while others return raw
vectors whose scale depends on the model architecture and training process.

Unexpectedly large values, such as coordinates around `100` or `1000`, are a
reason to investigate. They can indicate unstable training, exploding
activations, missing normalization, or a data-processing issue. They are not,
by themselves, proof that the model is broken: the correct test is whether
the values are abnormal for that particular model and representation.

## F011. An Embedding Table Is Not a Neural Network

An embedding table is a matrix used as a lookup table:

```text
token_id -> row vector
```

The table is not a neural network by itself. It does not run a sequence of
neuron-like calculations when retrieving a row. However, in an LLM it is a
trainable part of the larger neural network model. Its values are usually
updated through the same loss, backpropagation, and optimizer process as the
other trainable parameters. This is not a separate non-neural training rule.

There is no single invention date for lookup tables: they are a general
computing technique, not an invention specific to machine learning. Learned
vector representations also predate modern LLMs. For example:

- In 1990, Deerwester et al. published *Indexing by Latent Semantic Analysis*,
  which used vectors derived from a term-document matrix for information
  retrieval.
- At NIPS 2000, Bengio, Ducharme, and Vincent presented *A Neural
  Probabilistic Language Model*. Its expanded 2003 paper described a mapping
  from each vocabulary item to a real vector, represented as a matrix of free
  parameters learned jointly with the language model.

Therefore, the useful short version is: an LLM embedding table is not itself
a neural network, but it is a trainable parameter matrix inside a neural
network.

Sources:

- [Deerwester et al., 1990: *Indexing by Latent Semantic Analysis*](https://doi.org/10.1002/(SICI)1097-4571(199009)41:6%3C391::AID-ASI1%3E3.0.CO;2-9)
- [Bengio, Ducharme, Vincent, and Jauvin, 2003: *A Neural Probabilistic Language Model*](https://www.jmlr.org/papers/v3/bengio03a.html)
- [Bengio, Ducharme, and Vincent, NIPS 2000 version](https://papers.nips.cc/paper/1839-a-neural-probabilistic-language-model)

## F012. Attention Is an Operation, Not Just a Table

It is tempting to imagine an LLM as a simple chain:

```text
vector
-> table 1
-> table 2
-> table 3
-> output token
```

From this picture, a natural question appears: why do we call one of those
tables "attention"? Why not call it multiplication or any other arbitrary
name?

The answer is that attention is not the name of one arbitrary "second table."
It is the name of a specific computation. In self-attention, several learned
matrices create a query, key, and value vector for each token:

```text
Q = X * W_Q
K = X * W_K
V = X * W_V
```

The model then compares queries with keys, converts the comparison scores into
weights, and uses those weights to mix value vectors:

```text
Attention(Q, K, V) = softmax(Q * K^T / sqrt(d_k)) * V
```

For each token position, the resulting weights describe how strongly other
token positions should influence the new representation. For example, a token
may receive more information from a nearby adjective, a referenced noun, or an
earlier part of the sentence.

This is why the operation is called **attention**: it determines where the
model should draw information from at that moment. The learned matrices
`W_Q`, `W_K`, and `W_V` participate in the operation, but none of those
matrices alone is "the attention table."

The architecture was designed by researchers. Training does not independently
decide which matrix should become attention. Instead, researchers define the
attention computation, and training adjusts its matrices so that the complete
model predicts tokens more accurately.
