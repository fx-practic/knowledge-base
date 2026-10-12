Path: 004-rag-run/014-rag-evaluation-step.md

# 014. RAG Evaluation Step

RAG evaluation checks whether the retrieval system and final answer are useful.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Test questions, expected sources, retrieved chunks, and generated answers. | Measure retrieval and answer behavior. | Quality signals for improving the RAG system. |

## What this step means

RAG systems can fail in several places. Evaluation should test both retrieval
and generation.

## Retrieval evaluation

Retrieval questions:

```text
Did the system retrieve the right source?
Did it retrieve enough useful context?
Did it avoid irrelevant chunks?
Did it respect permissions and filters?
```

Common ideas:

| Metric idea | Meaning |
| --- | --- |
| Recall | Did the needed source appear in the retrieved set? |
| Precision | How many retrieved chunks were actually useful? |
| Rank quality | Did the best source appear near the top? |
| Freshness | Did retrieval prefer current documents over stale ones? |

## Answer evaluation

Answer questions:

```text
Does the answer use the retrieved context?
Is it factually supported by sources?
Does it cite the right source?
Does it avoid unsupported claims?
Does it say "not enough information" when sources are insufficient?
```

## Core conclusion

RAG quality is not only model quality. It depends on ingestion, chunking,
embedding, indexing, retrieval, reranking, context assembly, generation,
citations, and evaluation.

