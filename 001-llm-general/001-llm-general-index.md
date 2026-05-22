Path: 001-llm-general\001-llm-general-index.md

# LLM General

## 0. Terms

| Term | Short explanation |
| --- | --- |
| **Token** | A unit of text that an LLM reads and predicts. A token can be a full word, part of a word, punctuation, whitespace, byte sequence, or special control marker. |
| **Tokenizer** | The algorithm and files that convert text into token IDs and convert token IDs back into text. |
| **Vocabulary** | The standard term for the complete set of tokens a tokenizer can produce. It can also be called the tokenizer vocabulary or token vocabulary. |
| **Vocabulary size** | The number of possible token IDs in the tokenizer vocabulary. |
| **Token ID** | The numeric identifier assigned to a token. |
| **Embedding** | A learned vector representation of a token ID inside the model. |
| **Context size** | The maximum number of tokens a model can read in one prompt or conversation window. |
| **BPE** | Byte pair encoding, a common tokenizer training method that builds tokens from frequent character or byte sequences. |
| **SentencePiece** | A tokenizer framework often used for LLMs. It can implement methods such as BPE or Unigram. |
| **RAG** | Retrieval-augmented generation. A system pattern where relevant external information is retrieved and inserted into the prompt before the model answers. |
| **Fine-tuning** | Additional training on a specific dataset to adjust a model's behavior or knowledge. |

## 1. How Tokens Are Created Before LLM Training

Tokens are created before model training by training a tokenizer on a large text corpus.

The tokenizer learns a fixed vocabulary of common chunks, such as full words, word parts, punctuation, spaces, bytes, and special control tokens. After that, the LLM is trained to read and predict token IDs rather than raw text.

Most modern LLMs use a tokenizer method related to BPE, byte-level BPE, SentencePiece, or Unigram tokenization.

Example:

```text
unbelievable -> un + believ + able
```

A common word may be one token. A rare word, new word, typo, or technical term may be split into several smaller tokens.

## 2. What The Set Of Tokens Is Called

The correct general term is **vocabulary**.

More precise terms are:

- **Tokenizer vocabulary**: The full set of tokens known by a tokenizer.
- **Token vocabulary**: Another clear phrase for the same idea.
- **Vocabulary size**: The number of tokens in that set.

In LLM engineering, saying "this model has a 128k vocabulary" usually means the tokenizer has about 128,000 possible token IDs.

## 3. Are Tokens Reused Or Recreated For Each New Model?

Technically, both approaches are possible.

A company can reuse an existing tokenizer for a new model, or it can train a new tokenizer and vocabulary for a new model generation.

In regular practice, companies often reuse a tokenizer within a model family because it keeps compatibility between models, tools, prompts, datasets, fine-tuning pipelines, and evaluation systems.

But companies may create a new tokenizer when a new generation needs better multilingual coverage, better code handling, better compression, longer context efficiency, or a different chat/control-token format.

So the practical pattern is:

1. Reuse the tokenizer for nearby model versions inside the same family.
2. Redesign or retrain the tokenizer for larger generation changes.
3. Keep old tokenizers available because old models still depend on them.

## 4. Vocabulary Size In Leading Public LLMs

Vocabulary size means how many possible token IDs the model can represent. This is different from context size, which means how many tokens the model can read in one prompt.

Approximate public vocabulary sizes:

| Model family | Approximate vocabulary size | Notes |
| --- | ---: | --- |
| OpenAI GPT-4o class | About 200k | Public OpenAI tooling uses the `o200k_base` tokenizer family. |
| OpenAI GPT-4 / GPT-3.5 era | About 100k | Public OpenAI tooling uses the `cl100k_base` tokenizer family. |
| Meta Llama 3 / 3.1 | 128k | Meta publicly describes Llama 3 as using a 128K-token vocabulary. |
| Mistral modern Tekken tokenizer | 131k | Mistral documents a 131k vocabulary for Tekken. |
| Qwen | About 151k | Qwen documentation describes 151,643 regular BPE tokens, plus control tokens. |
| DeepSeek-V3 | About 128k to 129k | Public model configs and reports place it around this size. |
| Google Gemma | About 256k to 262k | Public Gemma configs use a large vocabulary in this range. |
| Google Gemini | Not fully public | Google exposes token counting, but not always the exact frontier tokenizer vocabulary. |
| Anthropic Claude | Not fully public | Anthropic exposes token counting, but not the full public tokenizer specification. |

These numbers are approximate because some companies publish exact tokenizer files while others expose only token-counting APIs.

## 5. Does A Bigger Vocabulary Mean A Better Model?

Not automatically.

A bigger vocabulary can be useful, but it is not direct evidence that a model is more intelligent, more capable, or more advanced overall.

A larger vocabulary can help with:

1. Better compression for many languages.
2. Fewer tokens for some words, names, code, or symbols.
3. More efficient handling of multilingual text.
4. Less fragmentation of rare words or technical terms.

But a larger vocabulary can also have tradeoffs:

1. Larger embedding and output layers.
2. More parameters spent on token representations.
3. More complexity in tokenizer design.
4. Possible inefficiency if many tokens are rarely used.

Model quality depends much more on architecture, training data, training compute, post-training, reinforcement learning, evaluation, tooling, and inference systems.

So a model with a 200k vocabulary is not automatically twice as advanced as a model with a 100k vocabulary.

The vocabulary is like the alphabet or dictionary available to the model. A larger dictionary can help, but it does not by itself make the writer smarter.

## 6. Can New Tokens Be Added After Training?

For closed API models, users generally cannot add new tokens.

For open-weight models, it is technically possible, but it is not a small edit. Adding tokens requires changing the tokenizer, resizing the model's input embedding table, resizing or adapting the output prediction head, initializing embeddings for the new tokens, and then continuing training or fine-tuning.

Without additional training, the model has no learned meaning for a new token.

So in normal production use, the tokenizer is treated as fixed after training.

## 7. How LLM Engineering Handles New And Obsolete Words

LLMs do not need every possible word to exist as a single vocabulary token.

New words can be split into smaller existing tokens or byte-level pieces.

Example:

```text
chatgptization -> chat + gpt + ization
```

If a word is very new, very rare, or written in an unusual form, it may cost more tokens, but the model can still read it.

Common engineering approaches are:

1. Keep the tokenizer fixed and let new words split into smaller pieces.
2. Continue training or fine-tune the model on newer text.
3. Use retrieval-augmented generation so the model can read fresh information at answer time.
4. Release a new model generation with a new tokenizer when needed.
5. Keep obsolete words in the vocabulary because removing token IDs would break compatibility with the trained weights.

## 8. Do Leading Companies Use The Same Tokens?

No. Leading AI companies usually have different tokenizers, vocabularies, special tokens, and chat formatting.

Even one company may use different tokenizers across model generations.

This means the same sentence can be split differently by OpenAI, Anthropic, Google, Meta, Mistral, Qwen, DeepSeek, and other model families.

## 9. What RAG Means

RAG means retrieval-augmented generation.

It is a way to help an LLM answer using external information instead of relying only on what was stored in its weights during training.

A simple way to think about it:

```text
LLM without RAG:
Question -> Model answers from training and prompt only.

LLM with RAG:
Question -> Search useful documents -> Put documents into prompt -> Model answers using those documents.
```

The basic flow is:

1. A user asks a question.
2. The system searches a knowledge base, document store, database, or web index for relevant material.
3. The most relevant passages are inserted into the model prompt.
4. The LLM writes an answer using both the user's question and the retrieved context.

Example:

```text
Question: What is our refund policy?
Retrieval: Find the latest refund policy document.
Generation: The LLM answers using that retrieved document.
```

The important point is that RAG does not permanently change the model.

It is more like giving the model a folder of papers to read before it answers. The model can use those papers in that answer, but the papers are not baked into the model's weights.

## 10. Why RAG Matters For New Words And New Knowledge

Tokenization handles new word spelling by splitting unfamiliar text into known pieces.

RAG handles new knowledge by retrieving current or private information and putting it into the prompt.

These solve different problems:

| Problem | Usual solution |
| --- | --- |
| New word spelling | Tokenizer splits it into known pieces. |
| New fact after training | RAG retrieves current information. |
| New company-specific data | RAG retrieves private documents or database records. |
| New skill or behavior | Fine-tuning or continued training may be needed. |

RAG is especially useful when the model must answer from information that changes often, such as company policies, documentation, prices, legal text, customer records, or recent events.

## 11. Tokenizer

A **tokenizer** is the component that turns raw text into tokens and token IDs.

It does three jobs:

| Job | Meaning |
| --- | --- |
| **Split** | Break raw text into tokens or token pieces. |
| **Encode** | Convert tokens into numeric token IDs. |
| **Decode** | Convert token IDs back into text. |

Simple flow:

```text
raw text -> tokenizer -> tokens -> token IDs
token IDs -> tokenizer -> text
```

A tokenizer does not simply convert whole words into tokens. It first decides how to split the text.

Example:

```text
unbelievable
```

may become:

```text
un + believable
```

or:

```text
un + believ + able
```

depending on the tokenizer.

Tokens can be:

| Token type | Example |
| --- | --- |
| Whole word | `hello` |
| Word part | `ing` |
| Space plus word | ` hello` |
| Punctuation | `.` |
| Number | `2026` |
| Code symbol | `()` |
| Byte piece | Part of an unusual symbol or unknown text. |
| Special marker | End-of-text, system message, user message, tool call marker. |

## 12. How Tokenizers Learn Splitting Rules

Tokenizer training is a **statistical algorithmic process**, not neural-network training.

No neural-network-style operation is used: no gradients, no backpropagation, no learned neural weights.

For example, BPE works roughly like this:

1. Start with very small pieces, such as characters or bytes.
2. Count which neighboring pieces appear together most often.
3. Merge the most frequent pair into a new token.
4. Repeat until the vocabulary reaches the chosen size.

Example:

```text
l + o appears often -> merge into lo
lo + w appears often -> merge into low
i + n + g appears often -> eventually becomes ing
```

The result is a vocabulary of useful text pieces.

This is "learning" only in a loose sense: the tokenizer extracts statistical patterns from text. It is not learning like the LLM itself learns.

## 13. Main Tokenizer Algorithms

| Algorithm | How it works |
| --- | --- |
| **Word-level** | Splits text by words. Simple, but bad with unknown words. |
| **Character-level** | Splits text into characters. Can represent anything, but creates many tokens. |
| **Byte-level** | Splits text into bytes. Can represent any text, but may be inefficient. |
| **BPE** | Starts with small pieces and repeatedly merges frequent pairs. |
| **Byte-level BPE** | BPE over bytes. Common in GPT-style tokenizers because it can handle almost any text. |
| **WordPiece** | Similar to BPE, but chooses pieces using a likelihood-based scoring method. |
| **Unigram** | Starts with many possible pieces, then removes less useful ones. Often used through SentencePiece. |

Modern LLMs usually use BPE, byte-level BPE, SentencePiece, Unigram, or related variants.

## 14. Are Tokenizer Algorithms Proprietary?

The main tokenizer algorithms are mostly open and standard.

| Algorithm or tool | Public status |
| --- | --- |
| **BPE** | Open, standard algorithm family. |
| **Byte-level BPE** | Open, standard algorithm family. |
| **WordPiece** | Publicly described and widely implemented. |
| **Unigram** | Publicly described and widely implemented. |
| **SentencePiece** | Open-source tokenizer framework. |
| **OpenAI tiktoken** | Open-source tokenizer library for OpenAI model encodings. |

But the exact tokenizer used by a company can still be proprietary or model-specific.

A tokenizer includes more than the algorithm:

| Part | Can differ by company/model? |
| --- | --- |
| Algorithm type | Yes, but usually based on known methods. |
| Vocabulary | Yes. |
| Merge rules | Yes. |
| Special tokens | Yes. |
| Chat formatting | Yes. |
| Implementation details | Yes. |

Short version:

```text
Algorithms are mostly open.
Exact production tokenizers are often custom.
```

## 15. Why Tokenizers Matter

Tokenizers affect how efficiently a model reads text.

| Area | Why it matters |
| --- | --- |
| **Cost** | Many APIs charge by token count. |
| **Context size** | More tokens means less text fits into the context window. |
| **Languages** | Some tokenizers handle some languages more efficiently than others. |
| **Code** | Code-friendly tokenizers can represent programming syntax more efficiently. |
| **New words** | Good fallback behavior lets the model read unknown words. |
| **Special tokens** | Chat models need markers for roles, tools, system messages, and boundaries. |

The tokenizer is not intelligent by itself.

```text
Tokenizer = splitting and mapping layer.
Model = learns and predicts patterns between token IDs.
```
