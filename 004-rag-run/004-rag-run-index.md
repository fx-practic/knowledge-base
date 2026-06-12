Path: 004-rag-run/004-rag-run-index.md

# 004. RAG Run Index

RAG means Retrieval-Augmented Generation.

RAG is a runtime system pattern where external information is retrieved and
placed into the model context before generation.

It is important for developers, but it is not only a developer trick. Production
LLM systems use retrieval to improve output quality without changing the base
model weights.

## Main flow

```text
source documents
-> ingestion
-> chunking
-> embedding
-> vector or search index
-> user query
-> retrieval
-> optional reranking
-> context assembly
-> generation with retrieved context
-> citations and evaluation
```

## Pages

| Number | Page | Purpose |
| --- | --- | --- |
| **005** | [Document Ingestion Step](./005-document-ingestion-step.md) | Load source documents and extract usable text and metadata. |
| **006** | [Chunking Step](./006-chunking-step.md) | Split documents into retrievable passages. |
| **007** | [Embedding Step](./007-embedding-step.md) | Convert chunks into vector representations for semantic search. |
| **008** | [Vector Index Step](./008-vector-index-step.md) | Store chunk vectors and metadata for fast retrieval. |
| **009** | [Retrieval Step](./009-retrieval-step.md) | Find candidate chunks for a user query. |
| **010** | [Reranking Step](./010-reranking-step.md) | Reorder retrieved candidates by likely usefulness. |
| **011** | [Context Assembly Step](./011-context-assembly-step.md) | Build the prompt context from selected chunks. |
| **012** | [Generation With Context Step](./012-generation-with-context-step.md) | Ask the LLM to answer using the retrieved context. |
| **013** | [Citations And Grounding Step](./013-citations-and-grounding-step.md) | Connect answer claims back to retrieved sources. |
| **014** | [RAG Evaluation Step](./014-rag-evaluation-step.md) | Test retrieval quality, answer quality, and source faithfulness. |

## RAG-specific difference

RAG does not normally train the base LLM.

Instead, it changes the information available during inference:

```text
same model weights
+ better retrieved context
= better grounded answer
```

This is why RAG is one of the main ways to improve LLM output for current,
private, or domain-specific knowledge.

