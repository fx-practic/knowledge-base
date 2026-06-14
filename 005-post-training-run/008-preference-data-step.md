Path: 005-post-training-run/008-preference-data-step.md

# 008. Preference Data Step

Preference data records which answer is better.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| A prompt and multiple candidate answers. | Humans or AI raters compare/rank answers. | Preference dataset. |

## What this step means

Instead of only showing one good answer, preference data compares answers:

```text
Prompt: Explain reinforcement learning.

Answer A: clear, correct, short.
Answer B: vague, too long, partly wrong.

Preferred: Answer A
```

This teaches the training system what kinds of answers are preferred.

The dataset can be described compactly as:

```text
(prompt, preferred answer, rejected answer)
```

This is not a special language for the LLM. It is a human-readable notation for
a training record. The same information may be stored in JSON, tables, or
another dataset format.

This step collects preference data. It does not by itself update the assistant
model weights.

## Core conclusion

Preference data turns human judgment into training signal.
