Path: 004-rag-run/012-generation-with-context-step.md

# 012. Generation With Context Step

The LLM generates an answer using the retrieved context.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| User question plus retrieved context. | Run normal LLM inference. | Draft answer grounded in supplied context. |

## What this step means

The generation step is still ordinary LLM inference:

```text
prompt with retrieved context
-> tokenizer
-> model forward pass
-> logits
-> sampler
-> output tokens
```

RAG changes what the model sees. It does not normally change the model weights.

## Why output can improve

The model can answer using information that was not stored in its parameters:

```text
new documents
private company knowledge
current product details
domain-specific rules
user-provided files
```

This is why RAG can improve answer quality even with the same base model.

## Core conclusion

RAG is a context-improvement method. It improves generation by supplying better
evidence at inference time.

