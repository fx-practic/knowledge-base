Path: 002-training-run/003-data-curation-step.md

# 003. Data Curation Step

This is the first training-run step.

## Term

The broad term for this step is **data curation**.

It includes collection, cleaning, filtering, deduplication, dataset mixing, and
formatting. "Collecting" alone is too narrow because raw collected data is not
ready for training.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Raw source data. | Collect, clean, filter, deduplicate, mix, and format examples for model training. | Training text corpus. |

## What this step means

Training starts before the neural network sees anything. The model does not
learn from the internet directly. Training software first prepares a corpus:
large text data that can later be tokenized.

## Typical sources

| Source type | Examples | Why it is useful |
| --- | --- | --- |
| Web pages | Articles, documentation, forums, general websites. | Broad language and world knowledge. |
| Books | Public-domain books, licensed books, textbooks. | Long-form structure and higher-quality prose. |
| Code | Git repositories, programming examples, documentation. | Programming syntax and software reasoning patterns. |
| Scientific and technical text | Papers, manuals, reference material. | Formal explanations and domain knowledge. |
| Dialogue data | Human conversations, instruction examples, assistant responses. | Teaches question answering and conversational format. |
| Curated datasets | Human-made or filtered benchmark-style examples. | Higher-quality targeted behavior. |

The exact sources depend on the model, license rules, company policy, and
training goal.

## Typical scale

The raw collected data can be enormous.

| Level | Rough scale |
| --- | --- |
| Small experimental model | Millions to billions of tokens. |
| Serious open model | Hundreds of billions to several trillion tokens. |
| Large frontier model | Often discussed in the trillion-token range or higher, but exact numbers may be private. |

Important distinction:

```text
raw collected data
!= final training corpus
```

The raw collection is filtered and reduced. Some data is removed because it is
duplicate, low quality, unsafe, private, broken, spammy, or not allowed by the
training policy.

## Common preparation operations

| Operation | What it does |
| --- | --- |
| Cleaning | Remove broken text, markup noise, boilerplate, encoding problems, and obvious junk. |
| Filtering | Remove low-quality, unsafe, private, or policy-disallowed material. |
| Deduplication | Remove exact or near-duplicate documents so the model does not over-train on repeated text. |
| Language detection | Keep or group text by language. |
| Quality scoring | Prefer clearer, more useful, or more trusted documents. |
| Mixing | Choose proportions between web, books, code, dialogue, and other sources. |
| Formatting | Convert many source formats into a consistent text-example format. |

## What the final corpus looks like

The final corpus is usually not one human-readable book. It is a huge collection
of text examples stored in dataset files.

Very simplified:

```text
example 1:
    text = "The quick brown fox jumps over the lazy dog."

example 2:
    text = "def add(a, b): return a + b"

example 3:
    text = "User: Explain attention.\nAssistant: Attention is ..."
```

Later, these text examples are tokenized:

```text
text examples
-> token IDs
-> training sequences
```

## Important idea

The corpus strongly affects what the model learns. Architecture and training
code matter, but the model can only learn patterns that are present in the
training data.

## Related page

See [Data Collection For LLM Training](../003-data-collection.md).
