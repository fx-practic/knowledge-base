Path: 003-inference-run/018-runtime-wrapper-and-stop-control-step.md

# 018. Runtime Wrapper And Stop Control Step

## Core idea

The neural network predicts tokens, but a real product needs surrounding
runtime logic.

This wrapper logic controls:

```text
prompt/chat formatting
streaming
context and output limits
stop rules
tool-call protocol
structured output
safety checks
logging
UI display
```

Some of this is done by the model runner. Some is done by the application that
uses the model runner.

## Where this step sits

Part of this wrapper happens before generation starts:

```text
messages
-> prompt template
-> system message
-> tools/schema/options
-> model call
```

Part of it happens after each token or output piece:

```text
chosen token
-> append token
-> stream output if needed
-> check stop rules and limits
-> continue or stop
```

So this page is not one neural-network layer. It describes the control layer
around inference.

## Model runner vs application

| Part | Usually handles |
| --- | --- |
| Model runner | Load model, tokenize, apply prompt template, run generation loop, manage KV cache, sample tokens, stream output, apply basic stop/limit options. |
| Application | RAG retrieval, permissions, real tool execution, safety/moderation policy, business rules, logging, audit, UI display, final formatting. |

## Main terms

| Term | Meaning |
| --- | --- |
| Prompt/chat formatting | Convert user/system/assistant messages into the exact text format expected by the model. |
| System message | Higher-priority instruction that guides model behavior. |
| Context limit | Maximum number of tokens the model/runtime can keep in the active input context. |
| Output limit | Maximum number of tokens the runtime allows the model to generate. |
| Stop sequence | Text/token pattern that tells the runtime to stop generation. |
| Streaming | Sending output pieces while generation is still running. |
| Structured output | Asking the model/runtime to return JSON or match a schema. |
| Tool calling | The model returns a requested tool call; the application usually executes the tool. |
| Safety policy | Rules and checks that decide what is allowed, blocked, logged, or escalated. |

## Ollama example

Ollama is a local model runner.

Ollama usually handles:

| Area | What Ollama can do |
| --- | --- |
| Model loading | Load and keep local models in memory. |
| Tokenization | Convert prompt text/messages into model token IDs. |
| Prompt template | Use model templates and system messages. |
| Sampling | Apply options such as temperature/top-p style settings. |
| Context limit | Use context-size options such as `num_ctx`. |
| Output limit | Use generation-length options such as `num_predict`. |
| Stop rules | Stop on configured stop sequences. |
| KV cache | Manage cached K/V attention vectors during generation. |
| Streaming | Return output as a stream of response objects unless disabled. |
| Structured output | Support JSON/schema-style output options. |
| Tool-call protocol | Return tool-call requests when tools are provided and the model supports them. |

Your application still usually handles:

| Area | What the application should do |
| --- | --- |
| RAG | Retrieve documents and assemble context before calling Ollama. |
| Permissions | Decide which user may see which documents or tools. |
| Tool execution | Actually call APIs, run code, search files, or perform actions. |
| Safety/moderation | Apply product rules before/after the model call. |
| Business rules | Enforce domain-specific rules that the model should not decide alone. |
| Logging/audit | Record requests, tool calls, errors, and important decisions. |
| UI display | Show streamed chunks, final answer, citations, errors, and controls. |

Important:

```text
Ollama runs the local model.
Ollama is not automatically a complete product safety system.
```

## Streaming

Streaming means the runtime sends output pieces while generation is still
running, instead of waiting for the full answer.

```text
token/text piece 1 -> client
token/text piece 2 -> client
token/text piece 3 -> client
```

Without streaming:

```text
model generates whole answer
-> runtime returns one final response
```

With streaming:

```text
model starts generating
-> runtime sends piece 1
-> runtime sends piece 2
-> runtime sends piece 3
-> final response/metadata arrives
```

Streaming is useful because the user can see progress before the full answer is
finished.

## Limits

There are two common limit types.

| Limit | Meaning |
| --- | --- |
| Context limit | How many tokens can fit in the active prompt/history/context. |
| Output limit | How many new tokens the runtime may generate. |

Example:

```text
context limit = input/history/RAG/tool messages budget
output limit = answer length budget
```

If the context is too large, the application or runtime must truncate,
summarize, reject, or remove some content.

## Stop control

Stop control decides when generation ends.

Common stop reasons:

| Stop reason | Meaning |
| --- | --- |
| End token | The model emits a special end token. |
| Stop sequence | The generated text matches a configured stop string. |
| Output limit | The runtime reaches the max generated-token count. |
| Tool call | The model asks for a tool and the app pauses normal text generation. |
| Application rule | The product decides the answer is complete or not allowed. |

## Formatting rules

Formatting can happen in several layers:

| Layer | Example |
| --- | --- |
| Prompt template | Convert chat messages into model-specific text. |
| System message | Tell the model the desired role and behavior. |
| Output instruction | Ask for bullets, JSON, short answer, or citations. |
| Structured output option | Ask runtime/model to follow JSON or a schema. |
| Application post-processing | Validate, display, or reject the returned format. |

Formatting instructions guide the model. They are not the same as guaranteed
business validation. Critical formats should be checked by code.

## Safety policies

Safety can come from more than one place:

| Layer | Role |
| --- | --- |
| Model training | The model may have safety behavior learned during post-training. |
| System prompt | The app can instruct the model about allowed behavior. |
| Runtime settings | The runner can apply stop rules or format limits. |
| Application checks | The product can block, redact, log, or escalate content. |

For local runners such as Ollama, do not assume there is a complete moderation
system unless the application adds one.

## Tool calls

Tool calling has two separate parts.

```text
model says: call tool X with arguments Y
application decides whether to allow it
application executes the tool
application sends result back to the model
```

The model runner may support the tool-call message format, but the application
is usually responsible for executing the tool safely.

## RAG

RAG is usually outside the model runner.

```text
user question
-> application retrieves documents
-> application builds context
-> runner sends context to model
-> model answers using that context
```

The model runner does not automatically know which private documents the user
is allowed to read. The application must enforce that.

## Core conclusion

The model predicts tokens.

The model runner handles inference mechanics.

The application handles product behavior.

For Ollama:

```text
Ollama = local model runner
your app = RAG, permissions, tools, strong safety, logs, UI
```

## Source notes

- Ollama API docs: https://github.com/ollama/ollama/blob/main/docs/api.md
- Ollama Modelfile docs: https://github.com/ollama/ollama/blob/main/docs/modelfile.mdx
