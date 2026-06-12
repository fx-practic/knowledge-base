Path: 004-rag-run/013-citations-and-grounding-step.md

# 013. Citations And Grounding Step

Citations connect answer claims back to retrieved sources.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Generated answer and retrieved source chunks. | Attach source references and check whether claims are supported. | Grounded answer with citations or source notes. |

## What this step means

RAG is often used because users need inspectable answers. A grounded answer
should make it clear which retrieved sources support the response.

Simple source format:

```text
[source 1] file path or URL, section heading
[source 2] file path or URL, section heading
```

The answer can then cite those sources.

## Grounding checks

Useful checks include:

```text
Does the answer use retrieved evidence?
Are important claims supported by a source?
Does the answer invent facts not in the context?
Are the citations matched to the correct claims?
Does the answer admit when the sources are insufficient?
```

## Core conclusion

Citations are not decoration. They are part of the quality control loop that
makes RAG more trustworthy than unsupported generation.

