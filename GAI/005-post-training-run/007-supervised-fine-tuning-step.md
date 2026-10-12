Path: 005-post-training-run/007-supervised-fine-tuning-step.md

# 007. Supervised Fine-Tuning Step

Supervised fine-tuning is often shortened to SFT.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Base model and instruction dataset. | Train on good prompt-answer examples. | Instruction-following model. |

## What this step means

SFT is still normal supervised training:

```text
prompt + good answer
-> predict answer tokens
-> loss
-> backpropagation
-> optimizer update
```

The model learns to imitate the style and structure of good assistant answers.

## Core conclusion

SFT is usually the first major post-training step. It teaches the base model to
respond like an assistant.

