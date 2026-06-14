Path: 005-post-training-run/005-post-training-run-index.md

# 005. Post-Training Run Index

Post-training is the stage after base pretraining where a model is shaped into
a more useful assistant.

Base training teaches broad next-token prediction. Post-training teaches
behavior:

```text
follow instructions
prefer better answers
refuse unsafe requests
use tools more reliably
answer in useful formats
```

## Main flow

```text
pretrained base model
-> instruction data
-> supervised fine-tuning
-> preference data
-> reward/preference model
-> RLHF or preference optimization
-> safety and behavior evaluation
-> assistant/chat model
```

## What becomes training data?

Post-training creates smaller, targeted training materials.

Examples:

```text
(prompt, good answer)
(prompt, preferred answer, rejected answer)
(prompt, generated answer, reward score)
```

This notation is not a special language that the LLM magically understands.
It is a compact way to describe dataset records. In real systems these records
may be stored as JSON, tables, text files, or another dataset format.

The important idea:

```text
006 and 008 collect training data
009 trains a separate reward/preference model
010 uses those materials/signals to update the LLM weights
```

So step 010 is different from step 008. Step 008 collects comparisons. Step 010
is a training step that uses the collected information to change the assistant
model's behavior.

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **006** | [Instruction Data Step](./006-instruction-data-step.md) | Collect examples of useful user instructions and assistant answers. |
| **007** | [Supervised Fine-Tuning Step](./007-supervised-fine-tuning-step.md) | Train the model to imitate good assistant answers. |
| **008** | [Preference Data Step](./008-preference-data-step.md) | Collect ranked or compared model answers. |
| **009** | [Reward Model Step](./009-reward-model-step.md) | Learn a scoring model from preference data. |
| **010** | [RLHF Step](./010-rlhf-step.md) | Use reinforcement learning from human feedback to improve behavior. |
| **011** | [Safety And Evaluation Step](./011-safety-and-evaluation-step.md) | Test helpfulness, safety, refusals, regressions, and task quality. |

## Post-training-specific difference

Base training mostly asks:

```text
what token is likely next?
```

Post-training asks:

```text
which answer is more useful, safe, and instruction-following?
```

Post-training can change model weights, but its goal is behavior shaping rather
than learning the whole world from scratch.
