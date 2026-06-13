Path: 022-questions-and-answers.md

# 022. Questions And Answers

## How Can Codex Understand Code By Reading Only Parts Of Files?

**Question.**

Code is very exact. One missing symbol can break compilation. How can Codex or
an LLM analyze code if it reads only part of a file instead of the whole
project at once?

**Answer.**

Codex does not fully understand a codebase from one random fragment alone.

It works by combining:

```text
local fragment
+ surrounding lines
+ search results
+ related definitions
+ compiler/test feedback
+ repeated inspection
```

Partial reading works because code is structured. Codex can search for symbols,
read the relevant function or class, inspect imports and helper definitions,
make a small patch, then run tests or a compiler.

Example workflow:

```text
search for relevant symbol
-> read nearby lines
-> read related definitions
-> edit small area
-> run tests/compiler
-> inspect errors
-> patch again if needed
```

So reading lines `201-280` does not mean the rest of the file is ignored
forever. It means those lines are the current working window. If they reference
another function, Codex can search for that function and read it too.

The important distinction is:

```text
file size on disk
!=
code currently loaded into the model context
```

Codex can work with large files by using tools to search, read chunks, patch
specific lines, and verify the result. It does not magically understand the
whole codebase at once.

## Core conclusion

Codex works more like a programmer with search, editor, and compiler feedback:

```text
read relevant context
-> make a small change
-> verify
-> inspect more if needed
```

---

## Are Visible Shell Commands All Actions Codex Uses?

**Question.**

Are commands such as `Get-Content`, `Select-Object`, `rg`, or `git status` all
the commands Codex uses to do a coding task? Or are they only the commands shown
to the user?

**Answer.**

They are not all internal actions. They are the visible **external tool
commands** Codex runs in the environment.

There are two different layers:

```text
1. Internal model work:
   reading, comparing, planning, deciding, reasoning

2. External tool commands:
   searching files, reading files, editing files, running tests, using git
```

Visible commands are shown because they interact with the user's machine or
repository:

```text
read file
search file
edit file
run tests
commit
push
```

Internal model work is not PowerShell, Bash, Python, or another shell language.
It is the model processing tokens and deciding what external action to take
next.

## How Many Actions Are There?

For a small coding task, Codex may run only a few external commands:

```text
search
read file
patch file
run test
git status
```

For a medium task, it may run dozens of external commands and tool calls:

```text
many searches
many file reads
several patches
several test runs
git checks
```

For a large debugging or refactoring task, the visible external commands can
reach dozens or sometimes more than a hundred.

But this is still very different from internal neural-network computation.
Inside the model, there are many mathematical operations, but they are not
separate shell commands like:

```text
Get-Content
rg
git status
```

So the practical answer is:

| Layer | Rough count in an average coding task |
| --- | --- |
| Visible external commands/tool calls | Usually several to dozens. |
| Large-task external commands/tool calls | Sometimes dozens to 100+. |
| Internal model computations | Very many math operations, but not user-visible commands. |

## Core conclusion

Visible commands are only the external tool actions. Codex also performs
internal model reasoning, but that reasoning is not a list of hidden PowerShell
commands.

---

## What Are Max Context Size And Padding?

**Question.**

What is max context size, what limits it, what are current model limitations,
and what is padding?

**Answer.**

Max context size means:

```text
maximum number of tokens the model/runtime can keep in one active context window
```

It includes:

```text
system prompt
+ chat history
+ user input
+ tool messages
+ generated output
```

So if a model has:

```text
context window = 4096 tokens
```

and the prompt/history already uses:

```text
3500 tokens
```

then only about:

```text
596 tokens
```

remain for output and overhead.

## What Limits Context Size?

| Limit | Meaning |
| --- | --- |
| Architecture/config | Model is built with a max position/context setting. |
| Position system | RoPE/positional encoding must support those positions. |
| Attention cost | Full attention grows roughly with `T x T`, so long context is expensive. |
| KV cache memory | During generation, stored keys/values grow with context length. |
| Training length | A model trained mostly on shorter contexts may behave worse on very long ones. |
| Product/runtime limit | API/app may allow less than the raw model can theoretically support. |

Context length is not only one number in config. It also affects memory, speed,
cost, and training quality.

## Current Model Limits

Public examples vary a lot:

| Model/platform | Public context size example |
| --- | ---: |
| GPT-4.1 API | About 1M tokens. |
| Gemini 1.5 Pro | Up to 2M tokens in some settings. |
| Claude models | Commonly 200k; some Sonnet versions advertise 1M beta/preview. |

Practical app limits may be smaller than model/API marketing numbers.

Sources:

- [OpenAI GPT-4.1](https://openai.com/index/gpt-4-1/)
- [Google Gemini long context](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/long-context)
- [Anthropic Claude Sonnet](https://www.anthropic.com/claude/sonnet)

## Padding

Padding means adding dummy tokens so multiple sequences have the same length
inside one batch.

Example:

```text
sample 1: [A, B, C, D]
sample 2: [E, F]
```

To batch them together:

```text
sample 1: [A, B, C, D]
sample 2: [E, F, PAD, PAD]
```

Shape becomes rectangular:

```text
B x T
```

But padding is not real content.

The model/runtime uses a mask:

```text
PAD tokens should not affect attention
PAD tokens should not count in loss
```

So padding is mainly a batching trick.

Important:

```text
Short input does not mean the model must process max_context tokens.
```

A single short inference request can be processed with actual length:

```text
T = 20
```

not necessarily padded to:

```text
T = 4096
```

Padding is more common when batching multiple samples/requests together.

---

## Where Is The Room For Improving LLM Quality?

**Question.**

If the transformer approach is commonly known and many companies use a similar
general approach, why are some companies' models better than others? At which
exact step does improvement happen? Do companies improve the model itself, or
do they mainly improve agents around the model?

**Answer.**

There is no single secret step where all quality improvement happens.

The public high-level approach can be similar:

```text
collect data
-> tokenize text
-> train transformer
-> predict next tokens
-> post-train for assistant behavior
-> run inference
```

But quality depends on many private engineering choices inside and around that
pipeline.

## Where The Model Itself Improves

The exact step where the model weights physically change is:

```text
loss
-> backpropagation
-> gradients
-> optimizer update
-> updated weights
```

So in the narrow technical meaning, the model improves during training when the
optimizer updates the parameters.

However, the optimizer can only improve the model according to the signal it
receives. That signal depends on many earlier choices:

| Area | Why it matters |
| --- | --- |
| Data curation | Better data gives the model better patterns to learn. |
| Data mixture | The company chooses how much web, code, books, dialogue, math, science, and other data to include. |
| Filtering | Removing spam, duplicates, unsafe text, broken text, and low-quality text changes what the model learns. |
| Architecture details | Companies may adjust model size, attention design, context length, normalization, activation functions, and other details. |
| Training recipe | Learning rate schedule, batch size, optimizer settings, and compute scale affect the final weights. |
| Evaluation | Better tests show which failures still need more training data, tuning, or design changes. |

So the real picture is:

```text
better data + better training recipe + better evaluation
-> better gradients
-> better optimizer updates
-> better model weights
```

## Post-Training

After base training, companies usually continue with post-training.

Post-training can include:

```text
supervised fine-tuning
preference training
reinforcement learning from human feedback
reinforcement learning from AI feedback
safety tuning
reasoning/tool-use tuning
```

This step often changes the visible behavior a lot.

Base training teaches broad language and knowledge patterns. Post-training
teaches the model to behave more like a useful assistant:

```text
follow instructions
answer in the requested format
refuse unsafe requests
use reasoning more carefully
ask clarifying questions when needed
use tools in a structured way
```

## Where Agents Improve Results Without Changing The Base Model

Sometimes the user experiences a better system even if the base model weights
are not changed.

This can happen through better agent design:

| System improvement | Effect |
| --- | --- |
| Retrieval | The system brings relevant documents into context. |
| Tool use | The model can search files, run code, use calculators, call APIs, or browse. |
| Planning loop | The system lets the model break work into steps and check progress. |
| Memory/context management | The system keeps useful context and drops less useful context. |
| Model routing | The product sends different tasks to different models. |
| Verification | The system runs tests, checks outputs, or asks another model to review. |

In this case, the underlying model may be the same, but the whole product works
better.

## Core distinction

There are two meanings of "better AI":

| Meaning | What improved? |
| --- | --- |
| Better model | The trained weights are better. |
| Better system | The model is surrounded by better tools, retrieval, memory, routing, and agent loops. |

Both matter.

The shortest answer is:

```text
Pretraining improves base capability.
Post-training improves behavior.
Inference systems and agents improve task performance.
```

## Core conclusion

The common transformer approach can be public while the quality difference
remains private.

Companies improve quality through data selection, data filtering, training
recipe, optimizer updates, post-training, evaluation, inference runtime, and
agent design.

The weights physically improve at the optimizer update step. The user-visible
system can also improve through agents and tools without changing the base
weights.

---

## Is RAG Only Important For Developers?

**Question.**

Is RAG important only for developers who build LLM applications, or is it also
used to improve model output quality in real AI systems?

**Answer.**

RAG is not only a developer topic.

RAG means Retrieval-Augmented Generation. It is one of the main ways an LLM
system can improve answers at runtime without retraining the base model.

The distinction is:

```text
training improves model weights
RAG improves what information the model sees before answering
agents/tools improve what the system can do around the model
```

RAG is especially useful when the answer should depend on:

```text
fresh information
private company knowledge
user-provided documents
domain-specific manuals
source citations
facts that were not in the model's training data
```

The basic flow is:

```text
user question
-> retrieve relevant documents
-> add them to the model context
-> generate answer using that context
```

So RAG is not a small side topic. It belongs beside training and inference as
one of the main practical patterns for modern LLM systems.

## Core conclusion

RAG normally does not permanently teach the model new facts. It improves output
by supplying external evidence during inference.

That makes it important both for developers and for production AI products.

---

## How Does RAG Work During Generation And Tool Calls?

**Question.**

How is RAG pronounced? In normal generation, is RAG content added only once
before generation starts? And in agentic RAG, what does it mean that "the model
decides it needs more information" if the model is only weights?

**Answer.**

RAG is usually pronounced like the English word:

```text
rag
```

Some people also spell it out:

```text
R-A-G
```

RAG means Retrieval-Augmented Generation.

## Simple RAG

In simple RAG, retrieval usually happens once before generation starts:

```text
user query
-> retrieve relevant documents or chunks
-> add retrieved text into the prompt/context
-> start generation
```

The retrieved text is not normally added again after every generated token.

Instead, the model generates token by token using the same context:

```text
token 1 uses: prompt + RAG context
token 2 uses: prompt + RAG context + token 1
token 3 uses: prompt + RAG context + token 1 + token 2
```

The RAG content stays available because it is part of the active context.

## Agentic RAG

In agentic RAG, retrieval can happen more than once.

Example:

```text
user query
-> retrieve initial sources
-> model begins response or reasoning
-> model emits a retrieval/tool-call request
-> application runs retrieval again
-> retrieved result is added back to context
-> model continues
```

This still does not mean retrieval happens after every token. It usually happens
at tool-call or search steps.

## What "model decides" really means

"The model decides" is shorthand.

More precisely:

```text
the model generates tokens that represent a tool-call or retrieval request
```

For example, the model may generate text shaped like:

```json
{"tool":"search","query":"latest OpenAI pricing"}
```

The model does not execute the search. The surrounding application or runtime
parses this output and decides whether to run the tool.

The real flow is:

```text
prompt says tools are available
-> model generates tokens
-> those tokens form a tool-call request
-> application parses the request
-> application executes the tool or retrieval
-> result is appended to context
-> model continues generation
```

So "model decides it needs more information" means:

```text
the model emitted a retrieval/tool-call request
```

not:

```text
the weights consciously decided something
```

## Why exact tool-call JSON can appear

The model can generate structured text because JSON is still just text tokens.

This:

```json
{"tool":"search","query":"latest OpenAI pricing"}
```

is generated token by token, just like:

```text
Hello world
```

The difference is that the application treats the structured output as a
command-like request instead of normal user-visible answer text.

## Core conclusion

Simple RAG usually retrieves once before generation.

Agentic RAG may retrieve again when the model emits a tool-call or retrieval
request.

The model generates the request as tokens. The application executes the tool.

---

## How Are Human Brains And LLMs Similar And Different?

**Question.**

Human brains often predict the future: a player predicts a ball trajectory, a
lawyer predicts an opponent's move, and an entrepreneur predicts market
behavior. LLMs also predict likely next tokens. If tools, memory, robots, and
feedback are added to an LLM, could it emulate human thinking and create new
knowledge?

**Answer.**

There is an important similarity:

```text
human thinking includes prediction
LLM generation includes prediction
```

Humans constantly estimate likely futures:

```text
where the ball will go
what another person will do
what argument will work
what action will cause which result
```

An LLM estimates likely next tokens:

```text
current context
-> probability distribution over next tokens
-> chosen next token
```

So humans and LLMs are not completely unrelated. Prediction is central in both.

## Main similarity

| Human brain | LLM |
| --- | --- |
| Predicts likely future states and actions. | Predicts likely next tokens. |
| Uses memory and context. | Uses prompt/context and trained weights. |
| Can use external tools. | Can be connected to tools by an application. |
| Can learn from feedback. | Can use feedback through context, memory, evaluation, or training. |

## Main difference

The difference is not just "probability vs no probability." Humans also use
probability-like prediction.

The bigger difference is the whole system around prediction.

| Human | LLM system |
| --- | --- |
| Living body with needs, pain, fatigue, emotions, hormones, and survival pressure. | Software model plus runtime; no biological body by default. |
| Continuous sensory stream from the world. | Receives only provided input unless connected to sensors/tools. |
| Persistent autobiographical memory. | Has limited context unless an external memory system is added. |
| Acts directly in the physical world. | Acts only through tools, APIs, robots, or applications. |
| Learns throughout life through many feedback loops. | Base weights are fixed during inference unless training/fine-tuning is added. |

Short version:

```text
LLM = prediction engine inside a software runtime
human = embodied control system that predicts, acts, feels, remembers, and survives
```

## Are human traits just more tools?

Many human-like abilities can be modeled as extra systems:

```text
memory
planning
vision
speech
robotic action
reward signals
risk checks
social feedback
long-term goals
```

If these are added around an LLM, the result becomes less like a plain chatbot
and more like an agent.

But this does not automatically prove it is a full human mind. It may emulate
many functions of intelligence without duplicating every biological detail.

## Can LLM systems create new knowledge?

Yes, in principle, if they are connected to verification.

For mathematics:

```text
LLM proposes theorem or proof
-> formal proof checker verifies it
-> new mathematical knowledge may be created
```

For science:

```text
observe data
-> form hypothesis
-> design experiment or simulation
-> compare result with prediction
-> update hypothesis
-> repeat
```

The LLM alone can generate ideas, but ideas are not automatically knowledge.
They become knowledge only after proof, experiment, or strong verification.

## What if LLMs are connected to robots and sensors?

If an LLM-like system is connected to:

```text
robots
laboratory tools
telescopes
scientific databases
simulators
symbolic math tools
theorem provers
long-term memory
planning systems
```

then it could become part of an autonomous discovery system.

The loop would be:

```text
observe
-> predict
-> act or experiment
-> receive feedback
-> revise plan or hypothesis
-> repeat
```

This is close to the scientific method.

## Is this backpropagation?

Not necessarily.

In normal LLM training, backpropagation changes model weights.

In an agentic discovery system, feedback may update:

```text
memory
notes
hypotheses
plans
tool choices
experiment design
```

If the system also retrains or fine-tunes the model, then feedback can update
weights too.

## Core conclusion

Human intelligence and LLM systems both involve prediction.

The main difference is that humans are embodied, persistent, self-maintaining
living systems, while LLMs are token-prediction models inside designed
runtimes.

Tool-augmented LLMs can emulate more human-like functions:

```text
memory
tools
sensors
robots
experiments
feedback
verification
```

Such systems could help create new knowledge, but generated ideas still need
proof, experiment, or verification before they count as knowledge.
