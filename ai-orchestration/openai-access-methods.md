Path: ai-orchestration/openai-access-methods.md

# OpenAI access methods: history and orchestration choices

Status date: **8 November 2026**.

Purpose: understand how OpenAI access methods developed and distinguish the options available when building a Python orchestrator.

The historical row identifiers **H1–H11** are preserved for follow-up discussion. This is a selected history of access methods for text and programming. It includes models, APIs, libraries, and applications; it is not an exhaustive list of OpenAI services.

## Terms used in this page

| Term | Meaning |
| --- | --- |
| Model | A neural network that generates responses, including possible requests to use tools. |
| HTTP API | A network interface your program calls to request a service. |
| SDK | A library that makes a service or runtime easier to use from code. Its capabilities depend on what it wraps. |
| Agent | A system combining a model, instructions, tools, and a loop that continues working toward a task. |
| Agent runtime / harness | Software that runs the agent loop and manages tool execution and working context. |
| CLI | A command-line interface to a program. |
| GUI | A graphical user interface to an application. |
| Orchestrator | Software that assigns work, coordinates agents, and decides what should happen next. |

A model can request a tool call. A runtime or external service executes the tool.

## Historical table

“Introduced” describes the initial appearance of the entry, not the launch date of every feature available today. Endpoint paths below identify the interfaces; they do not imply that today's exact URL structure was used at initial launch.

| ID | Introduced | What appeared | Category | How it is invoked | What it added and why | Status on 8 November 2026 |
| --- | --- | --- | --- | --- | --- | --- |
| H1 | 2020 | OpenAI API / text completions | Model access through an HTTP API | Send a prompt over HTTP; the legacy interface is known as `/v1/completions`. | Let a program request text continuation from a remote model. | **Legacy interface.** Original GPT-3 base models were retired on 4 January 2024. Endpoint and model retirement are separate facts. |
| H2 | 2021 | Original Codex models | Code-generation models | Select a Codex model in a model API request over HTTP. | Added models specialised in generating code, using the existing API access pattern. | **Historical models retired.** See the conflicting retirement entries in the H2 note below. |
| H3 | November 2022 | ChatGPT | Application / GUI | Open the website or app and enter a message. | Supplied a ready-made conversational interface and conversation management. | **Available product.** Features and model access depend on the account and plan. |
| H4 | March 2023 | Chat Completions API | HTTP API | Call `/v1/chat/completions`, directly or through an API SDK. | Accepted structured messages with roles. The client supplies the conversation context for subsequent calls. | **Supported.** Responses is the recommended starting point for new integrations. |
| H5 | November 2023 | Assistants API | HTTP API with managed assistant resources | Create assistants and threads, add messages, and start Runs. | Provided server-side assistant configuration, conversation state, and tool-related facilities. | **Retired on 26 August 2026.** Use Responses and appropriate conversation-state facilities instead. |
| H6 | March 2025 | Responses API | HTTP API | Call `/v1/responses`, directly or through an API SDK. | Unified model outputs and tool interactions. Supports continuation through response IDs and other conversation-state mechanisms. | **Available; recommended for new model integrations.** |
| H7 | March 2025 | OpenAI Agents SDK | Developer library / agent loop | Construct agents and run them from application code, for example with Python `Agent` and `Runner`. | Supplies reusable agent execution, tools, and handoffs inside your application. | **Available.** Distinct from the managed Agents API in H11. |
| H8 | April 2025 | Codex CLI | Command-line application / agent runtime | Run an interactive terminal session, or launch `codex exec` for a non-interactive task. | Provides a ready-made agent for tasks involving files, commands, and other configured tools. | **Available.** Can be launched by Python as a separate process. |
| H9 | May 2025 | Codex cloud agent | Hosted agent product / GUI access | Submit a task through the Codex cloud interface. | Executes work in a cloud working environment without running that task's agent on your PC. | **Available.** Cloud work does not automatically have access to your PC's filesystem. |
| H10 | From 2025; interfaces expanded later | Codex SDK / Codex app-server | Programmatic interfaces to the Codex runtime | Use a Codex SDK, or communicate with app-server through its protocol. | Lets another application control agent sessions, tasks, progress events, and, where supported, approvals. | **Available.** The current Python SDK controls local app-server over JSON-RPC; this does not mean that Python SDK existed in 2025. |
| H11 | 10 September 2026 | Agents API, public beta | HTTP API to a managed agent runtime | Create and run managed agent sessions through API requests. | OpenAI manages a Codex harness, session orchestration, context compaction, and recovery. Execution environments may be OpenAI-hosted or self-hosted. | **Public beta.** Account access and feature availability must be checked before adoption. |

Sources for API transitions and status: [Responses migration guide](https://developers.openai.com/api/docs/guides/migrate-to-responses), [API changelog](https://developers.openai.com/api/docs/changelog), and [retirement records](https://developers.openai.com/api/docs/deprecations). Product history and runtime interfaces: [ChatGPT and Codex changelog](https://learn.chatgpt.com/docs/changelog), [Codex SDK](https://learn.chatgpt.com/docs/codex-sdk), and [Codex app-server](https://learn.chatgpt.com/docs/app-server).

The early introduction periods are retained from the discussion's historical table. Current reference pages do not independently establish every original launch date.

### H2: original Codex retirement-date ambiguity

The official retirement page lists original Codex models under **23 March 2023**, but also lists `code-davinci-002` under **4 January 2024**. This page preserves that discrepancy rather than inventing a single unambiguous retirement date for the entire family.

These historical Codex models differ from the modern **Codex agent runtime**. A current model whose name includes “Codex” is also distinct from the application that runs an agent. [Retirement records](https://developers.openai.com/api/docs/deprecations)

## How to read the access mechanisms

### HTTP and Python describe different layers

For API rows, HTTP means a request to OpenAI's remote service. Python is one language that can send that request.

These two paths can reach the same endpoint:

```text
Python → HTTP client → Responses API
Python → OpenAI Python SDK → Responses API
```

The ordinary API SDK constructs requests and parses results. It does not, by itself, supply the same ready-made agent as the Codex runtime.

The **Agents SDK** adds an agent loop inside your application. The **Codex SDK** controls an existing Codex agent runtime. “SDK” alone does not tell you which architecture you are using.

### Local execution and remote inference are separate

A typical Codex CLI task using an OpenAI-hosted model follows this arrangement:

```text
Python orchestrator
  → Codex CLI process on the selected machine
      → remote model inference
      → tools acting in the configured working environment
```

“Local” describes the location of the agent process and its tools. It does not mean the OpenAI model runs offline on your PC.

The selected machine can be a laptop or a server. App-server also supports remote client connections. Tool permissions determine which files, commands, and other resources the agent can use. [Codex app-server](https://learn.chatgpt.com/docs/app-server)

### Execution location and billing are separate

Codex CLI supports ChatGPT sign-in for subscription access and API-key sign-in for usage-based access. Running the agent on your machine does not determine which billing method applies.

Included Codex usage limits are not the same resource as API tokens or an API credit balance. Check the active authentication method and the relevant plan or API terms before estimating cost. [Codex authentication](https://learn.chatgpt.com/docs/auth)

## Practical choices for a Python orchestrator

An orchestrator can coordinate several ready-made agents without implementing their internal working loops. Alternatively, it can control individual model calls and implement the agent behaviour itself.

| Choice | What Python invokes | Who supplies the agent loop? | Useful when |
| --- | --- | --- | --- |
| Direct Responses API | HTTP or the OpenAI Python SDK | Your code, using any hosted facilities you choose | You want detailed control over model calls, tools, and workflow. |
| OpenAI Agents SDK | Python agent and runner objects | The SDK inside your application | You want reusable tools, agent definitions, and handoffs. |
| Codex CLI | A subprocess such as `codex exec --json` | The Codex runtime | You want a simple task-in/result-out integration with an existing agent. |
| Codex Python SDK / app-server | A library or JSON-RPC protocol | The Codex runtime | You want programmatic session control and progress events; app-server exposes deeper client integration. |
| Agents API | Requests to a managed service | OpenAI's managed Codex runtime | You want durable agent sessions while delegating runtime operation to OpenAI. |

This comparison separates architectural choices, not different neural networks. GUI applications provide human-facing access; they need not become the programmatic interface for your orchestrator. [Official agent runtime comparison](https://developers.openai.com/api/docs/guides/agents), [Codex SDK](https://learn.chatgpt.com/docs/codex-sdk), [non-interactive Codex CLI](https://learn.chatgpt.com/docs/non-interactive-mode)

## Terminology for future discussion

Use a qualified name whenever “Codex” could be ambiguous:

- **Historical Codex models** for the original code-generation models.
- **Codex GUI** for a graphical interface; specify desktop or cloud when relevant.
- **Codex CLI** for the command-line application.
- **Codex SDK** for a library controlling the runtime.
- **Codex app-server** for the runtime's application-integration interface.
- **Codex agent runtime / harness** for the software running the agent loop.

Also distinguish **ChatGPT**, the application, from **OpenAI models accessed through an API**. Calling a model from Python does not automatically include a ChatGPT application's conversation history, instructions, tools, or account integrations.
