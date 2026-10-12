Path: 004-rag-run/011-context-assembly-step.md

# 011. Context Assembly Step

Context assembly builds the model input from retrieved chunks.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Selected chunks, user query, instructions, and metadata. | Arrange evidence into a prompt/context block. | Model-ready prompt with retrieved context. |

## What this step means

The model does not read the vector database directly. The RAG system must place
selected text into the model context.

Simplified:

```text
system instructions
+ user question
+ retrieved passages
+ source metadata
= final model prompt
```

## Important choices

| Choice | Why it matters |
| --- | --- |
| Passage order | The model may use earlier or clearer evidence more strongly. |
| Context budget | Retrieved text must fit inside the available context window. |
| Source labels | Labels make citations and grounding easier. |
| Deduplication | Repeated chunks waste context space. |
| Conflict handling | Conflicting sources need careful instruction or selection. |

## Core conclusion

RAG retrieval is not enough by itself. The system must assemble the retrieved
evidence into a context the model can use correctly.

