Path: 004-rag-run/010-reranking-step.md

# 010. Reranking Step

Reranking reorders retrieved candidates by likely usefulness.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Candidate chunks. | Score candidates more carefully against the query. | Better ordered candidate list. |

## What this step means

The first retrieval step is often optimized for speed. It may return many
candidate chunks. A reranker can then compare the query and candidates more
carefully.

Simplified:

```text
top 50 retrieved chunks
-> reranker
-> best 5 to 10 chunks
```

## Why reranking helps

Vector similarity does not always equal answer usefulness.

Reranking can prefer chunks that:

```text
directly answer the question
contain exact entities from the query
come from more trusted sources
are newer
are less duplicated
fit the user's intent better
```

## Core conclusion

Reranking is an optional but common quality step. It improves the final context
by choosing better evidence from the retrieved candidates.

