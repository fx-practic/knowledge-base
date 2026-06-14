Path: 005-post-training-run/009-reward-model-step.md

# 009. Reward Model Step

A reward model learns to score candidate answers.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Preference data. | Train a model to predict which answers humans prefer. | Reward or preference model. |

## What this step means

The reward model estimates answer quality:

```text
prompt + answer
-> reward model
-> score
```

The score is not objective truth. It is an approximation of the preference data
used to train it.

The reward model is trained separately from the assistant model.

It learns from records like:

```text
(prompt, preferred answer, rejected answer)
```

After training, it can score new candidate answers:

```text
(prompt, generated answer)
-> reward score
```

## Core conclusion

The reward model converts many human comparisons into a reusable scoring
function.
