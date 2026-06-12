Path: 004-rag-run/007-embedding-step.md

# 007. Embedding Step

The embedding step converts chunks into vectors for semantic search.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Text chunks. | Embedding model maps each chunk to a numeric vector. | Chunk vectors plus metadata. |

## What this step means

In RAG, embeddings are usually produced by a separate embedding model, not by
the main answer-generating model.

Simplified:

```text
chunk text
-> embedding model
-> vector
```

The vector is a coordinate representation. Similar meanings should have vectors
that are close to each other according to the search metric.

## Important distinction

RAG embeddings are not the same thing as the LLM's internal token embedding
table.

| Type | Purpose |
| --- | --- |
| LLM token embedding table | Converts token IDs into internal vectors inside the model. |
| RAG embedding model | Converts chunks or queries into vectors for search. |

## Query embeddings

At question time, the user's query is embedded too:

```text
user query
-> embedding model
-> query vector
```

The system then searches for chunk vectors close to the query vector.

## Core conclusion

Embeddings make semantic retrieval possible. They let the system retrieve text
that is related by meaning, not only by exact keyword match.

