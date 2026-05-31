`Path: 001-llm-general\001-003-model-files.md`

# 001-003. What An LLM Model Is Built From And What Files Store It

Related pages:

- [001. LLM General Index](./001-llm-general-index.md)
- [001-000. Glossary](./001-000-glossary.md)
- [001-002. Tokens](./001-002-tokens.md)

Related glossary terms:

- [Attention](./001-000-glossary.md#attention)
- [Checkpoint](./001-000-glossary.md#checkpoint)
- [Config](./001-000-glossary.md#config)
- [Embedding](./001-000-glossary.md#embedding)
- [Feed-Forward Network](./001-000-glossary.md#feed-forward-network)
- [Generation Config](./001-000-glossary.md#generation-config)
- [Model Architecture](./001-000-glossary.md#model-architecture)
- [Model Folder](./001-000-glossary.md#model-folder)
- [Output Layer](./001-000-glossary.md#output-layer)
- [Positional Encoding](./001-000-glossary.md#positional-encoding)
- [Safetensors](./001-000-glossary.md#safetensors)
- [Tensor](./001-000-glossary.md#tensor)
- [Tokenizer](./001-000-glossary.md#tokenizer)
- [Transformer Layer](./001-000-glossary.md#transformer-layer)
- [Vocabulary](./001-000-glossary.md#vocabulary)
- [Weights](./001-000-glossary.md#weights)

---

## 1. Final Outcome First

After creating an LLM, the final saved result is usually a **model folder**.

That folder mainly contains:

| Part | Typical file |
| --- | --- |
| Model structure | `config.json` |
| Tokenizer and vocabulary | `tokenizer.json` |
| Tokenizer settings | `tokenizer_config.json` |
| Special tokens | `special_tokens_map.json` |
| Trained numeric arrays | `model.safetensors` |
| Optional generation defaults | `generation_config.json` |

For large models, `model.safetensors` can be split into several shard files. Conceptually, it is still the same group of trained numeric arrays.

---

## 2. Creation Pipeline: Process And Result

| Process | Result |
| --- | --- |
| Collect text | Text corpus |
| Build tokenizer | Tokenizer files and vocabulary |
| Define model architecture | Model config |
| Create training samples from tokenized text | Input sequences and target sequences |
| Train the trainable arrays, including initial random values | Trained numeric arrays |
| Save final model | Model folder / checkpoint |

This page is about **creation**, not usage.

Important terminology:

| Term | Meaning |
| --- | --- |
| Token | piece of text |
| Training sample | input example made from a sequence of token IDs |
| Target / label | correct next token or correct next-token sequence |
| Parameter | one learned number inside the neural network |
| Trainable array | matrix, vector, or tensor containing parameters that are changed during training |

Avoid saying **training tokens** when the meaning is **training samples made from token sequences**.

---

## 3. Model Parts, Physical Form, And Corresponding Files

Let:

| Symbol | Meaning |
| --- | --- |
| `V` | vocabulary size |
| `d` | vector size / hidden size |
| `L` | number of transformer layers |
| `d_ff` | feed-forward internal size |
| `C` | context size |

Main model entities:

| Model part / entity | Corresponding file | Physical form | Typical shape | Meaning |
| --- | --- | --- | --- | --- |
| Model architecture | `config.json` | configuration values | no trained array | Defines layers, hidden size, attention heads, context length, vocabulary size |
| Tokenizer rules | `tokenizer.json` | tokenizer data / rules | not a neural matrix | Defines how text is split into tokens |
| Vocabulary | `tokenizer.json` or sometimes `vocab.json` | lookup table | about `V` entries | Token to token ID table |
| Special tokens | `special_tokens_map.json` | small mapping table | few entries | BOS, EOS, PAD, UNK, and similar control tokens |
| Tokenizer settings | `tokenizer_config.json` | configuration values | no trained array | Extra tokenizer parameters |
| Embedding table | `model.safetensors` | 2D matrix | `V x d` | Token ID to vector table |
| Positional embeddings, if learned | `model.safetensors` | 2D matrix | `C x d` | Learned token order representation |
| Formula-based positional system | `config.json` | formula/config | no learned matrix | Token order representation, for example RoPE-style position logic |
| Attention Q weights | `model.safetensors` | 2D matrix | `d x d` | Creates query vectors for attention |
| Attention K weights | `model.safetensors` | 2D matrix | `d x d` | Creates key vectors for attention |
| Attention V weights | `model.safetensors` | 2D matrix | `d x d` | Creates value vectors for attention |
| Attention output weights | `model.safetensors` | 2D matrix | `d x d` | Mixes attention output back into model vector space |
| Feed-forward weights | `model.safetensors` | 2D matrices | `d x d_ff`, `d_ff x d` | Internal neural network transformations |
| Normalization weights | `model.safetensors` | 1D vector | `d` | LayerNorm or RMSNorm scaling parameters |
| Output prediction matrix | `model.safetensors` | 2D matrix | `d x V` or `V x d` | Converts final hidden vector into token scores |
| Generation defaults | `generation_config.json` | configuration values | no trained array | Optional default generation parameters |

Most internal model parts are **not separate files**. They are usually stored together inside the trained weights file.

Embedding table shape:

```text
vocabulary_size x vector_size
```

Example:

```text
200,000 x 4,096
```

The embedding table has **one row per vocabulary token**. It does **not** have one row per training sample.

---

## 4. Training Samples, Targets, And Joint Training

In classic machine learning, training data often has:

| Input sample | Target / label |
| --- | --- |
| features | correct result |

In text model creation, the sample is usually made from token IDs.

Example:

```text
input sequence:  token_1, token_2, token_3, ... token_N
target sequence: token_2, token_3, token_4, ... token_N+1
```

The model receives the **input sequence** and tries to predict the **target sequence**.

Then one error signal is calculated.

That same error signal updates many trainable arrays together:

- embedding table
- attention matrices
- feed-forward matrices
- normalization vectors
- output prediction matrix

This does **not** mean these are separate models.

They are different parameter groups inside one connected neural network.

The forward calculation is sequential:

```text
token IDs
-> embedding table
-> positional information
-> transformer layers
-> output prediction matrix
-> predicted next-token scores
```

The parameter update is joint:

```text
one batch
one prediction task
one error signal
many trainable arrays updated
```

---

## 5. Why One `model.safetensors` File Can Store Many Parts

`model.safetensors` is a **container file**.

It does not contain one matrix. It contains many named tensors.

A **tensor** is an array of numbers. It may be:

- a vector
- a matrix
- a higher-dimensional numeric block

Simplified example:

```text
model.safetensors
|
|-- model.embed_tokens.weight
|-- layers.0.self_attn.q_proj.weight
|-- layers.0.self_attn.k_proj.weight
|-- layers.0.self_attn.v_proj.weight
|-- layers.0.mlp.gate_proj.weight
|-- layers.0.input_layernorm.weight
|-- layers.1.self_attn.q_proj.weight
|-- ...
|-- lm_head.weight
```

Each tensor inside the file has:

| Stored item | Meaning |
| --- | --- |
| Name | Tensor name, for example `layers.0.self_attn.q_proj.weight` |
| Shape | Dimensions, for example `4096 x 4096` |
| Data type | For example `float16` or `bfloat16` |
| Raw numbers | Actual trained parameter values |

---

## 6. Attention Weights Are Not Attention Scores

Important distinction:

| Term | Meaning |
| --- | --- |
| Attention matrices / attention weights | trained parameters stored in `model.safetensors` |
| Attention scores | temporary values calculated during one forward pass |

The stored attention parameters are matrices like:

```text
Wq
Wk
Wv
Wo
```

They create temporary vectors:

```text
Q = input x Wq
K = input x Wk
V = input x Wv
```

So attention weights are **trained matrices**, not a pre-made table of word relationships.

---

## 7. Example Mapping Inside `model.safetensors`

| Model part | Tensor name example | Stored in |
| --- | --- | --- |
| Embedding table | `model.embed_tokens.weight` | `model.safetensors` |
| Attention Q matrix | `layers.0.self_attn.q_proj.weight` | `model.safetensors` |
| Attention K matrix | `layers.0.self_attn.k_proj.weight` | `model.safetensors` |
| Attention V matrix | `layers.0.self_attn.v_proj.weight` | `model.safetensors` |
| Feed-forward matrix | `layers.0.mlp.gate_proj.weight` | `model.safetensors` |
| Normalization weights | `layers.0.input_layernorm.weight` | `model.safetensors` |
| Output layer | `lm_head.weight` | `model.safetensors` |

One file can store many named numeric tables.

---

## 8. How Config And Weights Work Together

`config.json` says:

```text
what parts must exist and what shape they have
```

`model.safetensors` contains:

```text
the actual trained numbers for those parts
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
config + tokenizer + trained arrays
```

---

## 9. Minimal Practical Model Folder

Example:

```text
my-llm/
    config.json
    model.safetensors
    tokenizer.json
    tokenizer_config.json
    special_tokens_map.json
```

Optional:

```text
    generation_config.json
```

For large models, the weight file can be split into shards, but this does not change the conceptual structure.

---

## 10. Approximate File Sizes

In a saved model folder, almost all size is in the trained weights file.

| File | Typical size | Notes |
| --- | ---: | --- |
| `config.json` | 10 KB - 300 KB | Model shape: layers, hidden size, attention heads, context length |
| `tokenizer.json` | 1 MB - 30 MB | Tokenizer rules and often vocabulary |
| `tokenizer_config.json` | 1 KB - 100 KB | Extra tokenizer settings |
| `special_tokens_map.json` | 1 KB - 20 KB | Special tokens such as BOS, EOS, PAD, UNK |
| `generation_config.json` | 1 KB - 50 KB | Optional default generation settings |
| `model.safetensors` | GB - TB | Trained arrays; usually more than 99.9% of total model size |

For large models, `model.safetensors` is often split into several shard files. This is a storage detail.

---

## 11. What Parameters Are

Parameters are **not tokens** and **not training samples**.

Simple definition:

```text
Parameter = one learned number inside the neural network.
```

Examples:

```text
0.1847
-1.3920
0.0063
```

A model contains billions of such numbers.

During the training process, these numbers are changed again and again until the model becomes good at predicting the next token.

---

## 12. Why The Weights File Is So Large

Approximate formula:

```text
model weight size = number of parameters x bytes per parameter
```

Typical precision sizes:

| Precision | Bytes per parameter | Example: 1T parameters |
| --- | ---: | ---: |
| FP32 | 4 bytes | about 4 TB |
| FP16 / BF16 | 2 bytes | about 2 TB |
| FP8 / INT8 | 1 byte | about 1 TB |
| 4-bit quantized | 0.5 byte | about 500 GB |

So a 70B model saved in BF16 is approximately:

```text
70B x 2 bytes = 140 GB
```

This is only the weight size. Full training checkpoints can be much larger because training may also store optimizer state, gradients, and other temporary training data.

---

## 13. Approximate Current Model Sizes

Exact sizes of closed frontier models are not public.

The table below gives approximate stored weight sizes by public or plausible model scale.

| Model / scale | Public parameter information | Approximate BF16/FP16 stored weight size | Notes |
| --- | ---: | ---: | --- |
| Small open LLM | 8B | about 16 GB | Common local model scale |
| Large open LLM | 70B | about 140 GB | Common high-quality open model scale |
| Llama 3.1 405B | 405B | about 810 GB | Public open-weight model scale |
| Qwen-style large MoE | about 235B total | about 470 GB | Total stored weights matter, not only active parameters |
| DeepSeek-V3 | 671B total / 37B active | about 1.34 TB | MoE model: only part is active per token, but full weights must be stored |
| Possible closed frontier dense model | unknown | hundreds of GB to several TB | Exact values are proprietary |
| Possible closed frontier MoE model | unknown | 1 TB to 10+ TB total stored weights | Total stored weights may be much larger than active weights per token |

Important distinction for MoE models:

```text
active parameters per token != total stored parameters
```

A MoE model may use only part of the model for each token, but the full saved model must still store all expert weights.

---

## 14. Key Distinction

| Thing | Meaning |
| --- | --- |
| Token | piece of text |
| Training sample | input example made from a token sequence |
| Target / label | correct next token or next-token sequence |
| Parameter | one learned number inside the neural network |
| Tensor | named array of numbers: vector, matrix, or higher-dimensional block |
| Architecture | empty structure of the model |
| Weights | learned numbers inside that structure |
| Tokenizer | converts text to token IDs and back |
| Config | tells software how to rebuild the structure |
| Checkpoint | saved model state |

---

## 15. Short Summary

| What | File |
| --- | --- |
| Structure | `config.json` |
| Language/token system | `tokenizer.json` and tokenizer config files |
| Learned numeric arrays | `model.safetensors` |
| Optional generation settings | `generation_config.json` |

The final saved model is not one simple file and not a database.

It is mainly **configuration + tokenizer + trained numeric arrays**.

The trainable arrays are physically different tensors, but they are not separate independent models. They are connected parts of one neural network and are updated together from the same prediction error.