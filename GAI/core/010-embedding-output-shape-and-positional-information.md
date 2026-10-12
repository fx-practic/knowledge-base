Path: core/010-embedding-output-shape-and-positional-information.md

# 010. Embedding Output Shape and Positional Information

After applying the **embedding table** to an input sequence, the result is a
**matrix**.

If:

```text
T = number of input tokens
D = embedding dimension
```

then the output shape is:

```text
T x D
```

Meaning:

```text
rows    = input tokens
columns = embedding dimensions
```

Example:

```text
Input: 5 tokens
D = 4096
Output: 5 x 4096 matrix
```

So the embedding table does not return one vector for the whole phrase. It
returns **one vector per token**.

## Position information

Token embeddings alone do not contain enough order information.

For example:

```text
dog bites man
man bites dog
```

contain the same words, but the order changes the meaning.

Therefore, the model needs **position information**.

## Classic positional embeddings

In the simplest version, the model adds a position vector to each token vector.

```text
token_vectors      shape: T x D
position_vectors   shape: T x D
--------------------------------
result             shape: T x D
```

Example with 2 tokens and 3 dimensions:

```text
token1 = [0.10, 0.20, 0.30]
token2 = [0.70, 0.80, 0.90]

pos1   = [0.01, 0.02, 0.03]
pos2   = [0.04, 0.05, 0.06]
```

After adding position information:

```text
token1_final = [0.11, 0.22, 0.33]
token2_final = [0.74, 0.85, 0.96]
```

The output is still:

```text
2 x D
```

It is **not doubled**. The model does not store:

```text
[token_vector, position_vector]
```

Instead it uses:

```text
token_vector + position_vector
```

## Other ways to add position information

| Method | Idea |
| --- | --- |
| **Learned position embeddings** | Trainable vector for position 1, 2, 3, etc. |
| **Sinusoidal encoding** | Fixed mathematical position vectors |
| **RoPE** | Rotates parts of vectors depending on position |
| **Relative position bias** | Adds bias based on distance between tokens |
| **ALiBi** | Adds distance penalty to attention scores |

## RoPE

**RoPE** means **Rotary Position Encoding**.

It does not add:

```text
token_vector + position_vector
```

Instead, it injects position information into attention by rotating pairs of
components in query and key vectors according to token position.

Very roughly:

```text
same token at position 1   -> relevant vector components rotated a little
same token at position 100 -> relevant vector components rotated more
```

This allows attention calculations to depend on the relative positions of
tokens.

## Core conclusion

After embedding and position processing, the model has:

```text
T x D matrix
```

where:

```text
T = number of tokens in the current input
D = fixed embedding dimension
```

The model can handle variable input length because **T can change**, while
**D stays fixed**.
