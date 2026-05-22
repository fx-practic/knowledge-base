# AGENTS.md

Repository: ai-com KB.

## Purpose
This repo is a structured knowledge base that turns a plain linear AI chat into a hierarchical tree. It allows clarifying details in separate branches/chats without losing the main structure.

## Main rules
- Work only in branch: `ai-com`.
- Do not use pull requests unless explicitly requested.
- Commit changes directly.
- Store everything as Markdown.
- First topic only: `LLM`.
- Use folders as topic branches.
- Use subfolders as clarification branches.
- Do not create `machine-learning/`, `python/`, or `git/` now.
- Do not rewrite unrelated files.

## Naming rules
- Every folder and file must start with a unique number.
- Folder names must include the parent folder name, separated by `-`.
- File names must include all parent folder numbers.
- Use lowercase names and dashes.

Example:

001-llm-general/
  001-llm-general-index.md
  001-001-learning-type/
    001-001-learning-type-index.md
    001-001-001-local-minimums.md

## Path rule
At the beginning of every Markdown file, add a copyable path line.

Example:

`Path: 001-llm-general\\001-001-learning-type\\001-001-001-local-minimums.md`

## Context rule
Before answering, analyse only the current folder/file and its parent chain up to the repository root. Do not analyse unrelated folders unless explicitly requested.

## Workflow rule
- After each user question, first answer in Codex chat only.
- Do not modify repository files automatically.
- Do not commit automatically.
- Add information to GitHub only after explicit user command.

## Commands that allow writing
- `save`
- `add to KB`
- `commit this`
- `write to GitHub`

If the user does not use one of these commands, treat the conversation as discussion only.
