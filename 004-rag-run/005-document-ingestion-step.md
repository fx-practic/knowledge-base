Path: 004-rag-run/005-document-ingestion-step.md

# 005. Document Ingestion Step

Document ingestion loads source material into the RAG system.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Files, web pages, database records, tickets, emails, or knowledge-base pages. | Read, extract text, clean obvious noise, and attach metadata. | Normalized document records. |

## What this step means

RAG starts before the user asks a question. The system first needs a searchable
collection of source documents.

Typical source material:

```text
PDF files
Markdown pages
HTML pages
Word documents
database rows
support tickets
API documentation
internal policies
product manuals
```

The important output is not only plain text. Metadata is also part of the
retrieval system:

```text
document title
source path or URL
author
creation date
section heading
access permissions
document type
```

## Common problems

| Problem | Why it matters |
| --- | --- |
| Bad text extraction | The model receives broken or missing evidence. |
| Lost headings | Retrieved chunks become harder to interpret. |
| Missing metadata | Citations, permissions, and filtering become weaker. |
| Duplicate documents | Search results repeat the same information. |
| Stale documents | The system may answer from outdated sources. |

## Core conclusion

RAG quality starts with source quality.

If ingestion produces noisy, incomplete, duplicated, or stale records, later
retrieval and generation steps inherit those problems.

