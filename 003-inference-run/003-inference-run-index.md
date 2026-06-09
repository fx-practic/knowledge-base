Path: 003-inference-run/003-inference-run-index.md

# 003. Inference Run Index

Inference is the run where a trained model is used to produce tokens.

This folder is ordered as a step-by-step path. The short step pages show the
operation order. Deeper shared explanations stay in `core/` to avoid copying
the same content into both training and inference.

## Main flow

```text
user prompt
-> tokenizer
-> inference runtime
-> forward pass through core model operations
-> logits
-> sampler
-> chosen token
-> append token
-> generation loop repeats
```

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **004** | [Prompt Tokenization Step](./004-prompt-tokenization-step.md) | Convert user text into token IDs. |
| **005** | [Inference Runtime Step](./005-inference-runtime-step.md) | Load model files and run the generation process. |
| **006** | [Embedding Lookup Step](./006-embedding-lookup-step.md) | Convert token IDs into vectors. |
| **007** | [Position Information Step](./007-position-information-step.md) | Add or inject token order information. |
| **008** | [Transformer Block Stack Step](./008-transformer-block-stack-step.md) | Pass vectors through fixed trained transformer blocks. |
| **009** | [Attention Step](./009-attention-step.md) | Mix information between allowed token positions. |
| **010** | [Causal Mask Step](./010-causal-mask-step.md) | Preserve left-to-right next-token behavior. |
| **011** | [Residual, Normalization, and MLP Step](./011-residual-normalization-mlp-step.md) | Finish the transformer block transformations. |
| **012** | [Output Logits Step](./012-output-logits-step.md) | Produce scores for possible next tokens. |
| **013** | [Sampler Step](./013-sampler-step.md) | Choose one token from logits or probabilities. |
| **014** | [KV Cache Step](./014-kv-cache-step.md) | Reuse previous key/value vectors during generation. |
| **015** | [Append Token Step](./015-append-token-step.md) | Add the selected token to the context. |
| **016** | [Generation Loop Step](./016-generation-loop-step.md) | Repeat next-token generation until a stop rule. |

## Inference-specific difference

Inference does not update weights. It uses the trained weights, chooses output
tokens, manages runtime memory such as KV cache, and stops according to runtime
rules.
