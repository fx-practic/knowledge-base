# AGENTS.md

Repository: ai-com KB.

## Purpose

This repo is a structured Markdown knowledge base for AI and LLM notes.

## Main rules

- Work only in branch: `ai-com`.
- Do not use pull requests unless explicitly requested.
- Commit changes directly.
- Store everything as Markdown.
- Current topic: `LLM`.
- Store global KB pages in the repository root.
- Store reusable model-operation pages in `core/`.
- Store training-specific run pages in `training-run/`.
- Store inference-specific run pages in `inference-run/`.
- Do not create `machine-learning/`, `python/`, or `git/` now.
- Do not rewrite unrelated files.

## Naming rules

- Every numbered Markdown page must start with a three-digit number.
- Page numbers are unique inside one folder. Different folders may each have their own `001` index page.
- Use lowercase names and dashes.
- Keep the numbering style already used in the repository.

Examples:

```text
001-general-index.md
002-glossary.md
003-data-collection.md
004-tokens.md
core/001-core-index.md
core/010-embedding-output-shape-and-positional-information.md
training-run/001-training-run-index.md
training-run/007-training.md
inference-run/001-inference-run-index.md
inference-run/012-generation-loop.md
```

## Path rule

At the beginning of every Markdown file, add a copyable path line.

Example:

```text
Path: 004-tokens.md
```

## Context rule

Before answering, analyse only the current file and closely related files. Do not analyse unrelated files unless explicitly requested.

## Workflow rule

- After each user question, first answer in chat only.
- Do not modify repository files automatically.
- Do not commit automatically.
- Add information to GitHub only after explicit user command.

## Commands that allow writing

- `save`
- `add to KB`
- `commit this`
- `write to GitHub`

If the user does not use one of these commands, treat the conversation as discussion only.
