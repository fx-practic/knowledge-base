Path: 002-training-run/002-training-run-index.md

# 002. Training Run Index

Training is the run where model parameters are changed.

This folder is ordered as a step-by-step path. The short step pages show the operation order. Deeper shared explanations stay in `core/` to avoid copying the same content into both training and inference.

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
| **006** | [Embedding Lookup and Mini-Batches Step](./006-embedding-lookup-and-mini-batches-step.md) | Convert token IDs into vectors and explain the `B x T x D` mini-batch shape. |
| **007** | [Position Information Step](./007-position-information-step.md) | Add or inject token order information. |
| **008** | [Transformer Block Stack Step](./008-transformer-block-stack-step.md) | Pass vectors through repeated transformer blocks. |
| **009** | [Transformer Block: Attention Step](./009-transformer-block-attention-step.md) | Inside each transformer block, mix information between allowed token positions. |
| **010** | [Transformer Block: Causal Mask Step](./010-transformer-block-causal-mask-step.md) | Inside attention, block future-token attention during next-token training. |
| **011** | [Transformer Block: Residual Add Step](./011-transformer-block-residual-add-step.md) | Add the block input back to the attention output. |
| **012** | [Transformer Block: Normalization Step](./012-transformer-block-normalization-step.md) | Stabilize numeric scale before the next sub-operation. |
| **013** | [Transformer Block: MLP Step](./013-transformer-block-mlp-step.md) | Transform each token row internally after attention. |
| **014** | [Output Logits Step](./014-output-logits-step.md) | Convert hidden vectors into vocabulary scores. |
| **015** | [Loss Step](./015-loss-step.md) | Compare logits with target token IDs. |
| **016** | [Backpropagation and Gradients Step](./016-backpropagation-gradients-step.md) | Calculate gradients using the computation graph. |
| **017** | [Optimizer Update Step](./017-optimizer-update-step.md) | Change model weights. |
| **020** | [Training Overview](./020-training-overview.md) | Older compact training overview. |
| **021** | [Trainable Tensors In LLMs](./021-training-tables-in-llms.md) | Deeper notes about trainable tensors, gradients, and updates. |

## Training-specific difference

Training does not stop after choosing a next token. It compares predictions with targets, calculates loss, runs backpropagation, and updates weights.
