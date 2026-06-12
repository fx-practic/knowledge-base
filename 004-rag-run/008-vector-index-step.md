Path: 004-rag-run/008-vector-index-step.md

# 008. Vector Index Step

The vector index stores chunk vectors for fast search.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Chunk vectors and metadata. | Store in a vector database, search index, or local index. | Searchable retrieval index. |

## What this step means

A RAG system may contain thousands, millions, or billions of chunks. Searching
every vector one by one can be too slow, so systems use an index.

The index stores:

```text
chunk ID
chunk text
embedding vector
source metadata
permissions
timestamps
```

## Index types

| Type | What it does |
| --- | --- |
| Vector index | Finds chunks with vectors close to the query vector. |
| Keyword index | Finds chunks by exact or lexical matches. |
| Hybrid index | Combines semantic vector search with keyword search. |

## Why metadata matters

Metadata allows the retrieval system to filter results:

```text
only documents user can access
only current documents
only selected product area
only selected language
only selected date range
```

## Core conclusion

The index is the searchable memory of a RAG system. Good indexes store both
vectors and the metadata needed to retrieve safely and precisely.

