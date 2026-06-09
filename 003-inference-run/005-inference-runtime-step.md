Path: 003-inference-run/005-inference-runtime-step.md

# 005. Inference Runtime Step

## Core idea

The model is not the same thing as the software that runs it.

Strictly:

```text
model = architecture + trained weights / parameters
```

To produce tokens, another piece of software must load the model and execute
the calculations. That software is called an **inference runtime**.

Related names:

```text
model runtime
LLM runtime
inference engine
serving runtime
inference server
```

## Examples

| Software | What it is |
| --- | --- |
| Ollama | LLM inference runtime / local inference server |
| llama.cpp | Inference engine / runtime |
| vLLM | Inference engine and serving runtime |
| TensorRT-LLM | Optimized inference runtime |
| Transformers + PyTorch | Framework/runtime combination used to execute inference |
| Hosted API backend | Remote inference/serving system |

## Main separation

| Thing | Role |
| --- | --- |
| Model architecture | Defines the computation structure. |
| Model weights | Store learned numeric parameters. |
| Tokenizer | Converts text to token IDs and token IDs back to text. |
| Inference runtime | Runs the model calculations and generation loop. |
| Sampler | Chooses the next token from logits/probabilities. |
| Application wrapper | Handles chat UI, API calls, tools, formatting, and policies. |

Short version:

```text
model weights are stored data
inference runtime is running code
```

## What the inference runtime does

| Step | Runtime responsibility |
| --- | --- |
| Load model | Read model files, config, tokenizer, and weights. |
| Prepare input | Tokenize prompt text into token IDs. |
| Run forward pass | Execute matrix operations through the model. |
| Produce logits | Get next-token scores from the output layer. |
| Sample token | Choose the next token using decoding settings. |
| Append token | Add the chosen token to the current context. |
| Manage KV cache | Store and reuse key/value vectors during generation. |
| Check stop rules | Stop on end token, max tokens, stop sequence, or system rule. |
| Return output | Stream or return generated text to the user/application. |

## Relationship to the generation loop

The **generation loop** is one process inside the inference runtime.

```text
inference runtime
-> tokenizes prompt
-> runs forward pass
-> samples next token
-> appends token
-> repeats through generation loop
-> returns text
```

The model itself does not decide to call itself again. The inference runtime
does that.

## Relationship to forward pass

A forward pass is one execution of the model:

```text
current token IDs -> next-token logits
```

The inference runtime executes the forward pass.

During generation:

```text
forward pass
-> sample token
-> append token
-> forward pass again
-> sample token
-> append token
...
```

## Is the inference runtime part of the LLM?

It depends on the meaning of "LLM".

| Meaning | Answer |
| --- | --- |
| Strict model meaning | No. The runtime is separate from the model weights and architecture. |
| Product/system meaning | Yes. A usable LLM system includes a runtime around the model. |

Precise statement:

```text
The model contains learned parameters.
The inference runtime executes the model and controls generation.
```

## Core conclusion

An inference runtime is the software that turns a saved model into a usable
token-generating system.

```text
model files
+ tokenizer
+ inference runtime
+ sampler / generation loop
= usable LLM system
```

Ollama is an example of this kind of software. It is not the model itself; it
is software that runs the model.
