Path: 004-rag-run/006-chunking-step.md

# 006. Chunking Step

Chunking splits documents into retrievable passages.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Normalized documents. | Split text into passages with useful boundaries and metadata. | Chunks ready for embedding and indexing. |

## What this step means

A full document is often too large or too broad to retrieve as one unit. RAG
systems usually split documents into smaller chunks.

Example:

```text
document
-> section 1 chunk
-> section 2 chunk
-> section 3 chunk
```

Each chunk should be large enough to contain useful meaning, but small enough
to retrieve precisely.

## Common chunking choices

| Choice | Meaning |
| --- | --- |
| Fixed-size chunks | Split every N tokens or characters. |
| Overlap | Repeat some text between neighboring chunks to avoid losing context at boundaries. |
| Section-aware chunks | Split by headings, paragraphs, or document structure. |
| Metadata inheritance | Keep title, source, section, date, and permission data on each chunk. |

## Why chunking matters

Bad chunking can hurt both retrieval and generation.

| Problem | Result |
| --- | --- |
| Chunk too small | The passage may not contain enough context to answer. |
| Chunk too large | Retrieval becomes less precise and context window space is wasted. |
| Split in the wrong place | Important explanation is separated from the fact it explains. |
| No overlap | Relevant text near a boundary can be missed. |

## Core conclusion

Chunking is not just a storage detail. It decides what units of knowledge the
RAG system can retrieve.

