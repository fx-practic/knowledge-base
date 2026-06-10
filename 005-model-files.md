Path: 005-model-files.md

# 005. What An LLM Model Is Built From And What Files Store It

Related pages:

- [001. LLM General Index](./001-general-index.md)
- [002. Glossary](./002-glossary.md)
- [004. Tokens](./004-tokens.md)
- [007. Bridge From Classic MLP Training To LLM Training](./007-mlp-reader-bridge.md)

Related glossary terms:

- [Attention Projection Weights](./002-glossary.md#attention-projection-weights)
- [Checkpoint](./002-glossary.md#checkpoint)
- [Config](./002-glossary.md#config)
- [Embedding](./002-glossary.md#embedding)
- [Model Architecture](./002-glossary.md#model-architecture)
- [Model Folder](./002-glossary.md#model-folder)
- [Output Layer](./002-glossary.md#output-layer)
- [Parameter](./002-glossary.md#parameter)
- [Safetensors](./002-glossary.md#safetensors)
- [Trainable Tensor](./002-glossary.md#trainable-tensor)
- [Tokenizer](./002-glossary.md#tokenizer)
- [Weights](./002-glossary.md#weights)

---

## 1. Final outcome first

After creating an LLM, the final saved result is usually a **model folder**.

| Part | Typical file |
| --- | --- |
| Model structure | `config.json` |
| Tokenizer and vocabulary | `tokenizer.json` |
| Tokenizer settings | `tokenizer_config.json` |
| Special tokens | `special_tokens_map.json` |
| Trained numeric tensors | `model.safetensors` |
| Optional generation defaults | `generation_config.json` |

For large models, the weights can be split into shard files. Conceptually, they are still one group of trained numeric tensors.

---

## 2. Creation pipeline

| Process | Result |
| --- | --- |
| Collect and curate text | Training corpus |
| Build tokenizer | Tokenizer files and vocabulary |
| Define model architecture | Model config |
| Create samples from tokenized text | Input sequences and target sequences |
| Train parameters | Trained tensors |
| Save final result | Model folder / checkpoint |

Important terminology:

| Term | Meaning |
| --- | --- |
| Token | Piece of text |
| Training sample | Input example made from token IDs |
| Target / label | Correct next token or next-token sequence |
| Parameter | One learned number inside the neural network |
| Trainable tensor | Matrix, vector, or tensor containing parameters changed during training |

Avoid saying **training tokens** when the meaning is **training samples made from token sequences**.

---

## 3. Main model parts

Let:

| Symbol | Meaning |
| --- | --- |
| `V` | Vocabulary size |
| `D` | Model dimension / hidden size |
| `L` | Number of transformer layers |
| `D_ff` | Feed-forward internal size |
| `C` | Context size |

| Model part | File | Physical form | Typical shape | Meaning |
| --- | --- | --- | --- | --- |
| Model architecture | `config.json` | Configuration | no trained tensor | Defines layers, hidden size, heads, context length, vocabulary size |
| Tokenizer rules | `tokenizer.json` | Tokenizer data | not a neural matrix | Defines how text is split into tokens |
| Vocabulary | `tokenizer.json` | Lookup table | about `V` entries | Token to token ID table |
| Embedding table | `model.safetensors` | 2D trainable tensor | `V x D` | Token ID to vector table |
| Learned position embeddings, if used | `model.safetensors` | 2D trainable tensor | `C x D` | Learned token order representation |
| RoPE-style position system | `config.json` | formula/config | no learned matrix | Injects position information into attention |
| Attention projection weights | `model.safetensors` | 2D trainable tensors | often `D x D`, or per-head `D x d_head` | Create query, key, and value vectors |
| Attention output projection | `model.safetensors` | 2D trainable tensor | often `D x D` | Mixes attention head output back into model dimension |
| Feed-forward weights | `model.safetensors` | 2D trainable tensors | `D x D_ff`, `D_ff x D` | Internal MLP transformations |
| Normalization weights | `model.safetensors` | 1D trainable tensor | `D` | LayerNorm or RMSNorm scaling parameters |
| Output layer | `model.safetensors` | 2D trainable tensor | `D x V` or `V x D` | Converts final hidden vectors into logits |

Most internal model parts are **not separate files**. They are usually stored together inside the trained weights file.

The embedding table has **one row per vocabulary token**. It does **not** have one row per training sample.

---

## 4. Joint training

The forward calculation is sequential:

```text
token IDs
-> embedding table
-> positional information
-> transformer blocks
-> output layer
-> logits
```

The parameter update is joint:

```text
one batch
one prediction task
one loss signal
many trainable tensors updated
```

This does **not** mean these are separate models.
They are different parameter groups inside one connected neural network.

---

## 5. Why one weights file can store many parts

`model.safetensors` is a **container file**.
It can contain many named tensors, for example:

```text
model.embed_tokens.weight
layers.0.self_attn.q_proj.weight
layers.0.self_attn.k_proj.weight
layers.0.self_attn.v_proj.weight
layers.0.mlp.gate_proj.weight
layers.0.input_layernorm.weight
lm_head.weight
```

Each tensor has:

| Stored item | Meaning |
| --- | --- |
| Name | Tensor name |
| Shape | Tensor dimensions |
| Data type | For example `float16` or `bfloat16` |
| Raw numbers | Trained parameter values |

---

## 6. Attention projection weights are not attention weights

Important distinction:

| Term | Meaning |
| --- | --- |
| Attention projection weights | Learned model parameters such as `W_Q`, `W_K`, `W_V`, `W_O` |
| Attention scores | Temporary values calculated during one forward pass before softmax |
| Attention weights | Temporary `T x T` softmax distribution over allowed token positions |

Stored attention projection weights create temporary vectors:

```text
Q = input x W_Q
K = input x W_K
V = input x W_V
```

So stored attention projection weights are **trained matrices**, not a pre-made table of word relationships.

---

## 7. How config and weights work together

`config.json` says:

```text
what parts must exist and what shape they have
```

`model.safetensors` contains:

```text
the trained numbers for those parts
```

When loading a model, software usually does this:

```text
read config.json
-> build empty architecture
-> read model.safetensors
-> put trained tensors into the right places
```

So the final saved model is mainly:

```text
config + tokenizer + trained tensors
```

---

## 8. What parameters are

Parameters are **not tokens** and **not training samples**.

```text
Parameter = one learned number inside the neural network
```

A model contains many such numbers. During training, these numbers are changed again and again until the model becomes better at predicting the next token.

---

## 9. Core conclusion

A saved LLM is mainly:

```text
architecture config
+ tokenizer files
+ trained tensors
```

The config defines the structure. The tokenizer defines text-to-token conversion. The trained tensors contain the learned numeric parameters.