Path: 005-post-training-run/010-rlhf-step.md

# 010. RLHF Step

RLHF means Reinforcement Learning from Human Feedback.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Assistant model, prompts, and reward/preference signal. | Train the model to produce answers with higher reward. | Behavior-tuned assistant model. |

## What reinforcement learning means

General reinforcement learning:

```text
agent tries actions
-> receives reward or penalty
-> learns to choose actions with higher reward
```

For LLMs, the "action" is usually generating an answer.

Simplified RLHF:

```text
model generates answer
-> reward model scores answer
-> training updates model toward higher-scoring answers
```

## What data is used here?

RLHF uses the materials produced earlier:

```text
prompts
preference data
reward/preference model
generated candidate answers
reward scores
```

The training system may create records like:

```text
(prompt, generated answer, reward score)
```

Then it uses those scores as training signal to update the assistant model
weights.

This is why step 010 is not the same as step 008:

| Step | What happens |
| --- | --- |
| 008 Preference Data | Collect comparisons such as `(prompt, preferred answer, rejected answer)`. |
| 009 Reward Model | Train a separate model to score answers from those comparisons. |
| 010 RLHF | Use prompts, generated answers, and reward scores to update the assistant model. |

## Why RLHF is used

Base next-token prediction does not automatically produce:

```text
helpful answers
safe refusals
good instruction following
preferred tone
concise formatting
```

RLHF is one way to push model behavior toward those preferences.

## Core conclusion

RLHF is post-training that uses human feedback indirectly through a reward or
preference signal.
