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
| Trained weights | `model.safetensors` |
| Optional generation defaults | `generation_config.json` |

For large models, `model.safetensors` can be split into several shard files. Conceptually, it is still the same group of trained weights.

---

## 2. Creation Pipeline: Process And Result

| Process | Result |
| --- | --- |
| Collect text | Text corpus |
| Build tokenizer | Tokenizer files and vocabulary |
| Define model architecture | Model config |
| Train model, including initial random weights | Trained weights |
| Save final model | Model folder / checkpoint |

This page is about **creation**, not usage.

---

## 3. Model Parts And Corresponding Files

| Model part / entity | Corresponding file | Meaning |
| --- | --- | --- |
| Model architecture | `config.json` | Defines layers, hidden size, attention heads, context length, vocabulary size |
| Tokenizer rules | `tokenizer.json` | Defines how text is split into tokens |
| Vocabulary | `tokenizer.json` or sometimes `vocab.json` | Token to token ID table |
| Special tokens | `special_tokens_map.json` | BOS, EOS, PAD, UNK, and similar control tokens |
| Tokenizer settings | `tokenizer_config.json` | Extra tokenizer parameters |
| Embedding table | `model.safetensors` | Token ID to vector table |
| Positional system | `config.json` and sometimes `model.safetensors` | Token order representation |
| Transformer layers | `model.safetensors` | Main neural network weights |
| Attention weights | `model.safetensors` | Matrices used by attention |
| Feed-forward weights | `model.safetensors` | Internal neural network matrices |
| Normalization weights | `model.safetensors` | LayerNorm or RMSNorm parameters |
| Output prediction layer | `model.safetensors` | Converts final hidden vector into token scores |
| Generation defaults | `generation_config.json` | Optional default generation parameters |

Most internal model parts are **not separate files**. They are usually stored together inside the trained weights file.

---

## 4. Why One `model.safetensors` File Can Store Many Parts

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
| Raw numbers | Actual trained weight values |

---

## 5. Example Mapping Inside `model.safetensors`

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

## 6. How Config And Weights Work Together

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

So the final LLM is mainly:

```text
config + tokenizer + trained weights
```

---

## 7. Minimal Practical Model Folder

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

## 8. Approximate File Sizes

In a saved LLM folder, almost all size is in the trained weights file.

| File | Typical size | Notes |
| --- | ---: | --- |
| `config.json` | 10 KB - 300 KB | Model shape: layers, hidden size, attention heads, context length |
| `tokenizer.json` | 1 MB - 30 MB | Tokenizer rules and often vocabulary |
| `tokenizer_config.json` | 1 KB - 100 KB | Extra tokenizer settings |
| `special_tokens_map.json` | 1 KB - 20 KB | Special tokens such as BOS, EOS, PAD, UNK |
| `generation_config.json` | 1 KB - 50 KB | Optional default generation settings |
| `model.safetensors` | GB - TB | Trained weights; usually more than 99.9% of total model size |

For large models, `model.safetensors` is often split into several shard files. This is a storage detail. Conceptually, these shards still represent the same trained weights.

---

## 9. What Parameters Are

Parameters are **not tokens** and **not training samples**.

Simple definition:

```text
Parameter = one learned number inside the neural network.
```

Examples:

```text
0.1847
1.3920
0.0063
```

An LLM contains billions of such numbers.

During training, these numbers are changed again and again until the model becomes good at predicting the next token.

---

## 10. Why The Weights File Is So Large

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

## 11. Approximate Current Model Sizes

Exact sizes of closed frontier models are not public. For OpenAI GPT models and Anthropic Claude models, the public documentation gives model names, capabilities, context limits, pricing, and usage details, but not downloadable weight files or exact parameter counts.

The table below gives approximate stored weight sizes by public or plausible model scale.

| Model / scale | Public parameter information | Approximate BF16/FP16 stored weight size | Notes |
| --- | ---: | ---: | --- |
| Small open LLM | 8B | about 16 GB | Common local model scale |
| Large open LLM | 70B | about 140 GB | Common high-quality open model scale |
| Llama 3.1 405B | 405B | about 810 GB | Public open-weight model scale |
| Qwen-style large MoE | about 235B total | about 470 GB | Total stored weights matter, not only active parameters |
| DeepSeek-V3 | 671B total / 37B active | about 1.34 TB | MoE model: only part is active per token, but full weights must be stored |
| Possible closed frontier dense model | unknown | hundreds of GB to several TB | GPT/Claude-class exact values are proprietary |
| Possible closed frontier MoE model | unknown | 1 TB to 10+ TB total stored weights | Total stored weights may be much larger than active weights per token |

Important distinction for MoE models:

```text
active parameters per token != total stored parameters
```

A MoE model may use only part of the model for each token, but the full saved model must still store all expert weights.

---

## 12. Key Distinction

| Thing | Meaning |
| --- | --- |
| Architecture | Empty structure of the model |
| Weights | Learned numbers inside that structure |
| Tokenizer | Converts text to tokens and back |
| Config | Tells software how to rebuild the structure |
| Checkpoint | Saved trained model state |

---

## 13. Short Summary

| What | File |
| --- | --- |
| Structure | `config.json` |
| Language/token system | `tokenizer.json` and tokenizer config files |
| Learned model itself | `model.safetensors` |
| Optional generation settings | `generation_config.json` |

The final LLM is not one simple file and not a database.

It is mainly **configuration + tokenizer + trained weights**.
