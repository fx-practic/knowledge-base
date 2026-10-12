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
- Store training-specific run pages in `002-training-run/`.
- Store inference-specific run pages in `003-inference-run/`.
- Do not create `machine-learning/`, `python/`, or `git/` now.
- Do not rewrite unrelated files.

## Naming rules

- Every numbered Markdown page must start with a three-digit number.
- Page numbers are unique inside one folder.
- The main folder index order is `core/001`, `002-training-run/002`, then `003-inference-run/003`.
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
002-training-run/002-training-run-index.md
002-training-run/003-data-curation-step.md
003-inference-run/003-inference-run-index.md
003-inference-run/004-prompt-tokenization-step.md
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
