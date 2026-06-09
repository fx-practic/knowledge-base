Path: 002-training-run/002-training-run-index.md

# 002. Training Run Index

Training is the run where model parameters are changed.

This folder is ordered as a step-by-step path. The short step pages show the
operation order. Deeper shared explanations stay in `core/` to avoid copying
the same content into both training and inference.

## Main flow

```text
training data
-> tokenization
-> target token shift
-> forward pass through core model operations
-> logits
-> loss
-> backpropagation
-> optimizer update
-> changed weights
```

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **003** | [Data Curation Step](./003-data-curation-step.md) | Collect, clean, filter, deduplicate, mix, and format text before tokenization. |
| **004** | [Tokenization Step](./004-tokenization-step.md) | Convert text into token IDs. |
| **005** | [Target Token Shift Step](./005-target-token-shift-step.md) | Create input IDs and next-token targets. |
| **006** | [Embedding Lookup Step](./006-embedding-lookup-step.md) | Convert token IDs into vectors. |
| **007** | [Position Information Step](./007-position-information-step.md) | Add or inject token order information. |
| **008** | [Transformer Block Stack Step](./008-transformer-block-stack-step.md) | Pass vectors through repeated transformer blocks. |
| **009** | [Attention Step](./009-attention-step.md) | Mix information between allowed token positions. |
| **010** | [Causal Mask Step](./010-causal-mask-step.md) | Block future-token attention during next-token training. |
| **011** | [Residual, Normalization, and MLP Step](./011-residual-normalization-mlp-step.md) | Finish the transformer block transformations. |
| **012** | [Output Logits Step](./012-output-logits-step.md) | Convert hidden vectors into vocabulary scores. |
| **013** | [Loss Step](./013-loss-step.md) | Compare logits with target token IDs. |
| **014** | [Backpropagation and Gradients Step](./014-backpropagation-gradients-step.md) | Calculate gradients using the computation graph. |
| **015** | [Optimizer Update Step](./015-optimizer-update-step.md) | Change model weights. |
| **020** | [Training Overview](./020-training-overview.md) | Older compact training overview. |
| **021** | [Training Tables in LLMs](./021-training-tables-in-llms.md) | Deeper notes about trainable tables, gradients, and updates. |

## Training-specific difference

Training does not stop after choosing a next token. It compares predictions
with targets, calculates loss, runs backpropagation, and updates weights.
