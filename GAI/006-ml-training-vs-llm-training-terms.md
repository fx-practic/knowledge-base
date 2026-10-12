Path: 006-ml-training-vs-llm-training-terms.md

# 006. ML Training vs LLM Training Terms

Related pages:

- [001. LLM General Index](./001-general-index.md)
- [002. Glossary](./002-glossary.md)
- [004. Tokens](./004-tokens.md)
- [005. Model Files](./005-model-files.md)

Related glossary terms:

- [Input Sequence](./002-glossary.md#input-sequence)
- [Label](./002-glossary.md#label)
- [Prediction](./002-glossary.md#prediction)
- [Target](./002-glossary.md#target)
- [Token](./002-glossary.md#token)
- [Training Sample](./002-glossary.md#training-sample)

---

## 1. Core Term Mapping

| General ML term | LLM equivalent |
| --- | --- |
| Training sample | Text window / token sequence |
| Input features | Previous token IDs |
| Target / label | Next token ID or shifted next-token sequence |
| Prediction / output | Probability distribution over possible next tokens |
| Error / loss | Difference between predicted probabilities and the target |
| Parameters | Learned numbers updated during training |

A token is a piece of text. A training sample is an example used for learning. In LLM training, a training sample is usually a **sequence of token IDs** with a corresponding **target sequence**.

---

## 2. Classic Supervised Machine Learning Terms

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

## 3. LLM Training Terms

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

The target is created automatically by shifting the text sequence by one position.

---

## 4. Important Distinction

| Term | Meaning |
| --- | --- |
| Token | Piece of text |
| Token ID | Numeric identifier of a token |
| Training sample | Input example used for learning |
| Input sequence | Sequence of token IDs given to the model |
| Target / label | Correct next token or shifted token sequence |
| Prediction | Model output before comparison with the target |
| Parameter | Learned number inside the model |

---

## 5. Short Summary

| General idea | LLM version |
| --- | --- |
| Sample | Input sequence of token IDs |
| Answer / result column | Target sequence |
| Model output | Prediction over next token IDs |
| Learning signal | Error between prediction and target |

LLM training uses **training samples made from token sequences**.

Each input sequence has a target sequence.

The model predicts next tokens and updates its parameters from the error.
