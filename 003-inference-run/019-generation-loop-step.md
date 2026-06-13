Path: 003-inference-run/019-generation-loop-step.md

# 019. Generation Loop Step

## Core idea

An LLM neural network predicts **one next token** for the current context.

A multi-token answer is produced by a surrounding **generation loop**:

```text
run model
-> choose next token
-> append token to context
-> run model again
-> repeat until stop
```

The generation loop is the correct term for the control process that keeps
asking the model for one more token.

## What part does what?

| Part | Role |
| --- | --- |
| **LLM neural network** | Given the current context, outputs probabilities or logits for the next token. |
| **Sampler / decoder** | Chooses one token from the model output. |
| **Generation loop** | Appends the chosen token and calls the model again. |
| **Stop controller** | Decides when to stop. |
| **Chat/API wrapper** | Handles messages, tools, formatting, safety rules, streaming, and other product behavior. |

## Is the generation loop part of the LLM?

It depends on the meaning of **LLM**.

| Meaning of "LLM" | Is the generation loop inside it? |
| --- | --- |
| Strict meaning: trained neural network architecture + weights | No. The loop is outside the neural network. |
| Product/system meaning: model + tokenizer + sampler + runtime + chat wrapper | Yes. It is part of the larger LLM system. |

So the precise statement is:

```text
The neural network predicts one token.
The inference runtime uses a generation loop to produce many tokens.
```

## Step-by-step generation

| Step | Operation | Shape / object |
| --- | --- | --- |
| 1 | User sends prompt. | Text |
| 2 | Tokenizer converts prompt into token IDs. | `T` token IDs |
| 3 | Model runs a forward pass. | Next-token logits |
| 4 | Sampler chooses one token. | One token ID |
| 5 | Runtime appends the chosen token to the context. | `T + 1` token IDs |
| 6 | Stop condition is checked. | Stop or continue |
| 7 | If not stopped, the model is called again. | Repeat |

## Pseudocode

```text
tokens = tokenize(prompt)

while not stopped:
    logits = model(tokens)
    next_token = sample(logits)
    tokens.append(next_token)

return detokenize(tokens)
```

This is simplified. Real systems also use runtime wrapper logic for KV cache,
maximum token limits, streaming, tools, safety policies, and formatting rules.

## Why short and long answers are possible

The model receives the instruction in the context:

```text
give me a short answer
```

or:

```text
give me a long answer
```

That instruction changes the next-token probabilities at each step.

| User instruction | Effect on generation |
| --- | --- |
| "Give me a short answer" | Tokens that finish quickly become more likely. |
| "Give me a long answer" | Tokens that continue explanation become more likely. |

The generation loop itself does not understand the whole answer like a human
planner. It simply keeps asking for the next token until a stop condition is
met.

## Stop conditions

| Stop reason | Meaning |
| --- | --- |
| End token | The model emits a special end-of-text or stop token. |
| Max token limit | The runtime stops after a configured number of generated tokens. |
| Stop sequence | The runtime stops when specific text appears. |
| Tool or system rule | The surrounding application decides the response is complete. |

## Core conclusion

The important separation is:

```text
LLM neural network:
    predicts one next token

Generation loop:
    repeatedly calls the model to produce many tokens
```

A long answer is not generated all at once. It is a chain of many one-token
predictions controlled by the inference runtime.
