Path: 002-training-run/014-output-logits-step.md

# 014. Output Logits Step

## Purpose

The model converts final hidden vectors into raw vocabulary scores.

## Input form

| Object | Shape | Meaning |
| --- | --- | --- |
| Final hidden matrix | `T x D` | One final vector row per token position. |

With a mini-batch:

```text
B x T x D
```

## Operation

Apply the output projection to each token row.

```text
hidden states -> output projection -> logits
```

## Output form

| Object | Shape | Meaning |
| --- | --- | --- |
| Logits | `T x VocabSize` | Raw scores for possible next tokens at each position. |

With a mini-batch:

```text
B x T x D
-> B x T x VocabSize
```

## What changed physically

Each `D`-wide hidden vector became a `VocabSize`-wide score row.

Approximate sense: this step converts internal model features into one score
for every possible vocabulary token. Higher score means "more likely next
token" before probability processing.

## What did not change

Logits are not probabilities yet. Softmax or cross-entropy processing happens after this step.

## Related glossary terms

- [Hidden State](../002-glossary.md#hidden-state)
- [Logits](../002-glossary.md#logits)
- [Output Layer](../002-glossary.md#output-layer)
- [Vocabulary Size](../002-glossary.md#vocabulary-size)

## Related page

See [Output Layer, Logits, and Next Token](../core/017-output-layer-logits-and-next-token.md).
