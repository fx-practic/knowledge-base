Path: 004-rag-run/009-retrieval-step.md

# 009. Retrieval Step

Retrieval finds candidate chunks for a user query.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| User query and searchable index. | Embed the query, search the index, and apply filters. | Candidate chunks. |

## What this step means

When the user asks a question, the system searches for source chunks that may
help answer it.

Typical flow:

```text
user query
-> query embedding
-> vector search
-> metadata filters
-> top candidate chunks
```

Retrieval can use more than one search method:

```text
semantic vector search
keyword search
hybrid search
metadata filters
```

## Retrieval quality

Good retrieval should return chunks that are:

```text
relevant
specific
current
permitted for the user
not duplicated
not misleading without surrounding context
```

## Common problems

| Problem | Result |
| --- | --- |
| Low recall | The system misses the needed source. |
| Low precision | The system returns many irrelevant chunks. |
| Permission leak | The system retrieves documents the user should not see. |
| Stale source | The answer uses outdated information. |

## Core conclusion

Retrieval is where RAG either finds useful evidence or fails before generation
starts.

