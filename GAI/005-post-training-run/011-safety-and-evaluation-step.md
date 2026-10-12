Path: 005-post-training-run/011-safety-and-evaluation-step.md

# 011. Safety And Evaluation Step

Safety and evaluation check whether post-training improved behavior without
breaking important capabilities.

## Transformation

| Input | Operation | Output |
| --- | --- | --- |
| Post-trained model and test sets. | Run evaluations, red-team tests, behavior checks, and regression tests. | Quality and safety signals. |

## What this step means

Post-training can improve one behavior while damaging another. Evaluation looks
for both improvements and regressions.

Common checks:

```text
instruction following
truthfulness
refusal behavior
tool-use behavior
format following
coding/math quality
domain task quality
safety failures
over-refusal
```

## Core conclusion

Post-training is not finished when loss or reward improves. The model must be
tested against real behavior requirements.

