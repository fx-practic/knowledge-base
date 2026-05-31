`Path: 001-llm-general\001-004-ml-training-vs-llm-training-terms.md`

# 001-004. ML Training vs LLM Training Terms

Related pages:

- [001. LLM General Index](./001-llm-general-index.md)
- [001-000. Glossary](./001-000-glossary.md)
- [001-002. Tokens](./001-002-tokens.md)
- [001-003. Model Files](./001-003-model-files.md)

Related glossary terms:

- [Input Sequence](./001-000-glossary.md#input-sequence)
- [Label](./001-000-glossary.md#label)
- [Prediction](./001-000-glossary.md#prediction)
- [Target](./001-000-glossary.md#target)
- [Token](./001-000-glossary.md#token)
- [Training Sample](./001-000-glossary.md#training-sample)

---

## 1. Why This Page Exists

The phrase **training tokens** can be confusing.

A token is a piece of text. A training example is not just one token. In LLM training, a training example is usually a **sequence of token IDs** with the correct next token or shifted target sequence.

Better terms:

| General ML term | LLM equivalent |
| --- | --- |
| Training sample | Text window / token sequence |
| Input features | Previous token IDs |
| Target / label | Next token ID |
| Prediction / output | Probability distribution over next token |

---

## 2. Correct Wording

Instead of saying:

```text
training tokens
```

it is often clearer to say:

```text
training samples made from token sequences
```

or shorter:

```text
training sequences
```

The phrase **training tokens** is still used in the industry when people discuss the total amount of text used for training, for example:

```text
the corpus contains 10 trillion tokens
```

But when explaining the learning process, **training sample** or **input sequence** is clearer.

---

## 3. Classic Machine Learning Terms

In classic supervised machine learning:

| Part | Meaning |
| --- | --- |
| Input sample | Data provided to the model |
| Target / label | Correct answer used during training |
| Prediction | Model output |
| Error / loss | Difference between prediction and target |

Example:

| Input sample | Target / label |
| --- | --- |
| market features | price up / price down |
| image pixels | cat / dog |
| customer data | buys / does not buy |

During production, only the input sample is provided. The trained model calculates the result.

---

## 4. LLM Training Terms

Example text:

```text
I like green apples
```

After tokenization:

```text
[I, like, green, apples]
```

Training samples can be understood like this:

| Input sample | Target / label |
| --- | --- |
| `I` | `like` |
| `I like` | `green` |
| `I like green` | `apples` |

In real training, this is usually done in larger blocks:

```text
input sequence:  token_1, token_2, token_3, ... token_N
target sequence: token_2, token_3, token_4, ... token_N+1
```

So the model is not given a manually created answer column like in a small spreadsheet. The answer is created automatically by shifting the text sequence by one position.

---

## 5. Important Distinction

| Term | Meaning |
| --- | --- |
| Token | Piece of text |
| Training sample | Input example used for learning |
| Input sequence | Sequence of token IDs given to the model |
| Target / label | Correct next token or shifted token sequence |
| Prediction | Model output before comparison with the target |
| Parameter | Learned number inside the model |

---

## 6. Short Summary

Use these terms when possible:

| Prefer | Avoid when explaining learning |
| --- | --- |
| Training sample | Training token |
| Input sequence | Training token |
| Target / label | Answer column |
| Prediction | Model result |

The clearest wording is:

```text
LLM training uses training samples made from token sequences.
Each input sequence has a target sequence.
The model predicts next tokens and updates its parameters from the error.
```
