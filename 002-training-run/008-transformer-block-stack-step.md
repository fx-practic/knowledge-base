Path: 002-training-run/008-transformer-block-stack-step.md

# 008. Transformer Block Stack Step

The position-aware vectors pass through many transformer blocks.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Matrix `X`, shape `T x D`. | Apply transformer block 1, then block 2, and so on. | Updated matrix, shape `T x D`. |

With a mini-batch:

```text
B x T x D
-> B x T x D
```

## What this step means

Each block transforms the same table shape. The rows still correspond to token
positions, but the values become more context-aware after every block.

## Related page

See [Transformer Block After Attention](../core/016-transformer-block-after-attention.md).
