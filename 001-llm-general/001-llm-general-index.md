Path: 001-llm-general\001-llm-general-index.md

# LLM General

## 1. How Tokens Are Created Before LLM Training

Tokens are created before model training by training a tokenizer on a large text corpus.

The tokenizer learns a fixed vocabulary of common chunks, such as full words, word parts, punctuation, spaces, bytes, and special control tokens. After that, the LLM is trained to read and predict token IDs rather than raw text.

Most modern LLMs use a tokenizer method related to BPE, byte-level BPE, SentencePiece, or Unigram tokenization.

Example:

```text
unbelievable -> un + believ + able
```

A common word may be one token. A rare word, new word, typo, or technical term may be split into several smaller tokens.

## 2. Vocabulary Size In Leading Public LLMs

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

## 3. Can New Tokens Be Added After Training?

For closed API models, users generally cannot add new tokens.

For open-weight models, it is technically possible, but it is not a small edit. Adding tokens requires changing the tokenizer, resizing the model's input embedding table, resizing or adapting the output prediction head, initializing embeddings for the new tokens, and then continuing training or fine-tuning.

Without additional training, the model has no learned meaning for a new token.

So in normal production use, the tokenizer is treated as fixed after training.

## 4. How LLM Engineering Handles New And Obsolete Words

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

## 5. Do Leading Companies Use The Same Tokens?

No. Leading AI companies usually have different tokenizers, vocabularies, special tokens, and chat formatting.

Even one company may use different tokenizers across model generations.

This means the same sentence can be split differently by OpenAI, Anthropic, Google, Meta, Mistral, Qwen, DeepSeek, and other model families.

## 6. What RAG Means

RAG means retrieval-augmented generation.

It is a way to help an LLM answer using external information instead of relying only on what was stored in its weights during training.

The basic flow is:

1. A user asks a question.
2. The system searches a knowledge base, document store, database, or web index for relevant material.
3. The most relevant passages are inserted into the model prompt.
4. The LLM writes an answer using both the user's question and the retrieved context.

RAG is useful because it can provide newer, private, or very specific information without retraining the model.

Example:

```text
Question: What is our refund policy?
Retrieval: Find the latest refund policy document.
Generation: The LLM answers using that retrieved document.
```

RAG does not change the model's vocabulary or permanently teach the model new facts. It gives the model relevant context at the moment of answering.

## 7. Why RAG Matters For New Words And New Knowledge

Tokenization handles new word spelling by splitting unfamiliar text into known pieces.

RAG handles new knowledge by retrieving current or private information and putting it into the prompt.

These solve different problems:

| Problem | Usual solution |
| --- | --- |
| New word spelling | Tokenizer splits it into known pieces. |
| New fact after training | RAG retrieves current information. |
| New company-specific data | RAG retrieves private documents or database records. |
| New skill or behavior | Fine-tuning or continued training may be needed. |
