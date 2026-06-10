Path: 007-mlp-reader-bridge.md

# 007. Bridge From Classic MLP Training To LLM Training

Related pages:

- [001. LLM General Index](./001-general-index.md)
- [002. Glossary](./002-glossary.md)
- [005. Model Files](./005-model-files.md)
- [006. ML Training vs LLM Training Terms](./006-ml-training-vs-llm-training-terms.md)
- [002. Training Run Index](./002-training-run/002-training-run-index.md)
- [003. Inference Run Index](./003-inference-run/003-inference-run-index.md)

Related glossary terms:

- [Activation](./002-glossary.md#activation)
- [Backpropagation](./002-glossary.md#backpropagation)
- [Batch](./002-glossary.md#batch)
- [Forward Pass](./002-glossary.md#forward-pass)
- [Gradient](./002-glossary.md#gradient)
- [Hidden State](./002-glossary.md#hidden-state)
- [Logits](./002-glossary.md#logits)
- [Parameter](./002-glossary.md#parameter)
- [Trainable Tensor](./002-glossary.md#trainable-tensor)
- [Transformer Block](./002-glossary.md#transformer-block)

---

## 1. Core bridge

If you trained classic multi-layer perceptrons, the main idea is familiar:

```text
input numbers
-> learned weights
-> hidden transformations
-> output scores
-> loss
-> backpropagation
-> optimizer update
```

An LLM uses the same general neural-network training logic, but the input form is different:

```text
text
-> token IDs
-> token-vector matrix
-> transformer blocks
-> vocabulary logits
-> loss or sampler
```

The most important change is this:

```text
Classic MLP often processes one feature vector.
LLM transformer usually processes a token-position matrix: T x D.
```

---

## 2. Term mapping

| Classic MLP concept | LLM / transformer equivalent |
| --- | --- |
| Input vector | Token ID sequence, then token matrix `T x D` |
| Feature columns | Embedding dimensions / hidden dimensions |
| Hidden layer | Transformer block or sublayer |
| Weight matrix | Parameter matrix / trainable tensor |
| Activation | Activation or gating inside the MLP/feed-forward part |
| Output scores | Logits over vocabulary tokens |
| Class label | Target next-token ID |
| Loss | Usually cross-entropy over next-token prediction |
| Backpropagation | Same chain-rule idea through transformer operations |
| Optimizer update | Updates all trainable tensors together |

---

## 3. Physical data form

Classic MLP simplified shape:

```text
one sample: feature_count
batch:      B x feature_count
```

LLM simplified shape:

```text
one sequence: T token IDs
batch:        B x T token IDs
embedding:    B x T x D
```

Where:

| Symbol | Meaning |
| --- | --- |
| `B` | batch size |
| `T` | number of token positions in the current sequence |
| `D` | model dimension / hidden size |
| `V` | vocabulary size |

---

## 4. LLM training flow in MLP language

| Step | Data form | MLP-style interpretation |
| --- | --- | --- |
| Text corpus | text examples | Raw source data before numeric conversion |
| Tokenization | `B x T` token IDs | Convert symbolic input into numeric IDs |
| Embedding lookup | `B x T x D` | Convert each ID into a learned feature vector |
| Transformer blocks | `B x T x D` | Hidden layers that transform token rows |
| Output projection | `B x T x V` | Scores for every possible next token at each position |
| Loss | scalar | Error measure against shifted target tokens |
| Backpropagation | gradients | Chain-rule calculation through the full graph |
| Optimizer | updated parameters | Weight update step |

---

## 5. Forward pass versus training update

Forward pass:

```text
current parameters + input token IDs
-> logits
```

Training update:

```text
logits + target token IDs
-> loss
-> gradients
-> optimizer update
-> changed parameters
```

The forward pass creates predictions. Backpropagation and the optimizer change the parameters.

---

## 6. Inference flow

Inference does not update weights:

```text
prompt
-> token IDs
-> forward pass
-> next-token logits
-> sampler chooses one token
-> append token
-> repeat
```

The trained neural network predicts one next-token distribution at a time. The inference runtime runs the generation loop around it.

---

## 7. Main differences from a simple MLP

| Difference | Meaning |
| --- | --- |
| Input is a sequence | The model keeps one row per token position. |
| Attention creates a `T x T` table | Token positions compare with token positions. |
| Same weights are reused across token rows | The parameter size does not grow when `T` grows. |
| Output is over vocabulary | The model predicts token IDs, not one small fixed class list. |
| Training target is shifted text | The correct answer is usually the next token from the same sequence. |
| Inference uses a loop | Long text is generated one token at a time. |

---

## 8. Core conclusion

A transformer LLM is still a trainable neural network.

The practical mental model is:

```text
Classic MLP:
    feature vector -> hidden layers -> output scores

LLM transformer:
    token matrix -> transformer blocks -> vocabulary logits
```

The training principle is the same family of ideas: forward pass, loss, gradients, and optimizer updates.

The main difference is that the LLM works with **token-position rows** and predicts **the next token**.