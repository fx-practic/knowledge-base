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
- Store current KB pages in the repository root.
- Do not create `machine-learning/`, `python/`, or `git/` now.
- Do not rewrite unrelated files.

## Naming rules

- Every Markdown page must start with a unique number.
- Use lowercase names and dashes.
- Keep the numbering style already used in the repository.

Examples:

```text
001-llm-general-index.md
001-000-glossary.md
001-001-data-collection.md
001-002-tokens.md
```

## Path rule

At the beginning of every Markdown file, add a copyable path line.

Example:

```text
Path: 001-002-tokens.md
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
