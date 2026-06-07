Path: inference-run/018-sampler-temperature-top-k-top-p.md

# 018. Sampler, Temperature, Top-k, and Top-p

## Core idea

The neural network produces logits for the next token.

The **sampler** decides which token is actually selected.

```text
logits
-> sampling rules
-> chosen next token
```

The sampler is part of the inference runtime, not the trained neural network
weights.

## Input and output

| Object | Shape / form | Meaning |
| --- | --- | --- |
| Logits | `1 x Vocab` | Raw score for every possible next token. |
| Probabilities | `1 x Vocab` | Softmax-normalized distribution. |
| Chosen token | `1 token ID` | The next token appended to the context. |

## Greedy decoding

Greedy decoding chooses the highest-probability token.

| Candidate | Probability |
| --- | ---: |
| `the` | 0.60 |
| `dog` | 0.20 |
| `.` | 0.10 |

Greedy result:

```text
the
```

Greedy is predictable, but it can be repetitive or too narrow.

## Temperature

Temperature changes how sharp or flat the probability distribution is.

| Temperature | Effect |
| --- | --- |
| Low, such as `0.2` | More deterministic; high-probability tokens dominate. |
| Around `1.0` | Normal sampling strength. |
| High, such as `1.5` | More random; lower-probability tokens get more chance. |

Shape does not change:

```text
logits:        1 x Vocab
probabilities: 1 x Vocab
```

Only the values change.

## Top-k

Top-k keeps only the `k` highest-scoring tokens and removes the rest.

Example:

| Candidate | Probability before top-k | Kept with `k = 3`? |
| --- | ---: | --- |
| `the` | 0.45 | yes |
| `dog` | 0.25 | yes |
| `.` | 0.15 | yes |
| `banana` | 0.03 | no |
| `carpet` | 0.02 | no |

Then probabilities are renormalized over the kept tokens.

## Top-p / nucleus sampling

Top-p keeps the smallest set of tokens whose combined probability reaches a
threshold `p`.

Example with `p = 0.90`:

| Candidate | Probability | Cumulative | Kept? |
| --- | ---: | ---: | --- |
| `the` | 0.45 | 0.45 | yes |
| `dog` | 0.25 | 0.70 | yes |
| `.` | 0.15 | 0.85 | yes |
| `runs` | 0.07 | 0.92 | yes |
| `banana` | 0.03 | 0.95 | no |

Top-p adapts the number of candidate tokens to the distribution shape.

## Stop behavior

The sampler can choose a special end token if the model gives it enough
probability.

Generation can also stop because of runtime rules:

| Stop rule | Meaning |
| --- | --- |
| End token | Model selected a stop/end token. |
| Max tokens | Runtime limit reached. |
| Stop sequence | Runtime detected a configured text sequence. |

## Core conclusion

The model produces the next-token distribution.

The sampler chooses from that distribution.

```text
model = scores possible next tokens
sampler = chooses one next token
generation loop = appends it and repeats
```
