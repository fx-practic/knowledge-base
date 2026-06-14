Path: 005-post-training-run/006-instruction-data-step.md

# 006. Instruction Data Step

Instruction data contains examples of user requests and good assistant answers.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Prompts, tasks, human-written answers, edited answers, synthetic examples. | Collect and filter examples of desired assistant behavior. | Instruction-following dataset. |

## What this step means

The base model can predict text, but it may not naturally behave like a helpful
assistant.

Instruction data shows examples such as:

```text
User: Explain RAG briefly.
Assistant: RAG means Retrieval-Augmented Generation...
```

## Core conclusion

Instruction data gives the model examples of the behavior humans want.

