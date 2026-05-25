# Curriculum: LangChain and LlamaIndex Agent Frameworks

## Tier 1 — Prerequisites

- ✅ LLM API basics — messages, system prompts, tool calls, structured output, streaming, retries, and model selection.
- ✅ Python service engineering — virtual environments, async basics, logging, tests, type hints, and dependency pinning.
- 📖 RAG basics — embeddings, chunking, vector search, query engines, and retrieval evaluation.
- 📖 Agent patterns — ReAct, tool calling, planner/executor, orchestrator, evaluator, and human-in-the-loop.
- 📖 Observability — traces, spans, run metadata, eval sets, and cost/latency tracking.
- 🔬 State machines and workflow orchestration — actually study if rusty; agent frameworks become clearer when compared to explicit state.

## Tier 2 — Core

### Tool Contract

- **Definition**: A typed interface that exposes a deterministic function to a model or workflow.
- **Leverage**: Good tool contracts make framework migration possible and prevent agents from guessing hidden behavior.
- **Source**: LangChain tool docs; LlamaIndex FunctionTool and FunctionAgent docs.
- **Maps onto**: Public API design: schema, semantics, errors, and side effects matter more than the decorator.

### Agent Loop

- **Definition**: A feedback loop where a model chooses a tool, observes the result, and repeats until it emits a final answer.
- **Leverage**: Useful for unpredictable tasks, but risky when control flow or side effects must be strict.
- **Source**: LangChain agents docs and LlamaIndex FunctionAgent docs.
- **Maps onto**: A scheduler whose policy is partly learned and partly prompted.

### LangChain `create_agent`

- **Definition**: LangChain's high-level agent abstraction for binding models, tools, middleware, and the agent loop.
- **Leverage**: Fast path for standard agents with many model/tool integrations.
- **Source**: LangChain agents documentation.
- **Maps onto**: A web framework's controller abstraction: quick to start, with escape hatches when complexity grows.

### LangGraph Runtime

- **Definition**: A lower-level orchestration framework and runtime for long-running, stateful, durable agent workflows.
- **Leverage**: Gives explicit control over state, edges, persistence, interrupts, and human approval.
- **Source**: LangChain docs on frameworks/runtimes and LangGraph workflows/agents docs.
- **Maps onto**: Temporal or workflow engines, except some transitions are model-driven.

### LlamaIndex `FunctionAgent`

- **Definition**: A function-calling agent abstraction that wires an LLM to tools.
- **Leverage**: Gives a concise way to build data-oriented tool agents inside the LlamaIndex ecosystem.
- **Source**: LlamaIndex "Building an agent" docs.
- **Maps onto**: A focused application service for tool-backed question answering.

### LlamaIndex `AgentWorkflow`

- **Definition**: A workflow abstraction for one or more agents, including handoffs among specialist agents.
- **Leverage**: Useful when multiple data-oriented specialists collaborate and handoff logic can remain mostly framework-managed.
- **Source**: LlamaIndex multi-agent patterns documentation.
- **Maps onto**: A team of services coordinated by a shared routing protocol.

### LlamaIndex Workflows

- **Definition**: Event-driven, step-based execution where steps consume and emit events.
- **Leverage**: Makes agentic applications explicit without abandoning LlamaIndex's data and retrieval strengths.
- **Source**: LlamaIndex Workflows documentation.
- **Maps onto**: Event-driven architecture with typed events and handlers.

### Observability and Evals

- **Definition**: Recording model calls, tool calls, state transitions, retrieved context, and final outputs for debugging and measurement.
- **Leverage**: Without traces and evals, framework demos cannot become production systems.
- **Source**: LangSmith observability docs and LlamaIndex instrumentation docs.
- **Maps onto**: Distributed tracing plus regression tests for probabilistic systems.

## Tier 3 — Advanced / Applied

- Middleware and hooks: dynamic prompts, retries, call limits, summarization, and human approval.
- Persistence: checkpoints, thread state, memory, resume semantics, and cross-session behavior.
- Multi-agent patterns: swarm/handoff, orchestrator, planner, reviewer, and agents-as-tools.
- Retrieval integration: query engines as tools, source fetching, metadata filtering, and response citation.
- Streaming: partial responses, intermediate events, and UI-friendly traces.
- Guardrails: tool-call limits, prompt-injection handling, schema validation, output parsers, and approval gates.
- Deployment: dependency pinning, API version churn, long-running workers, queues, and rollback.
- Migration: neutral tool contracts, trace schemas, and evals that survive framework replacement.

## Tier 4 — Frontier

- Durable long-running agents with resumable state and human interrupts.
- Multi-agent orchestration beyond demos: ownership, handoff contracts, and failure recovery.
- Agent observability standards, including OpenTelemetry-style traces for LLM calls and tools.
- Model Context Protocol integration as a way to standardize tool access across clients.
- Compiling stable workflows into smaller tuned models or deterministic services.
- Framework convergence: high-level agents built on lower-level runtimes, and data frameworks adding workflow engines.
- Agent security: prompt injection, tool output poisoning, supply-chain risk in integrations, and secrets isolation.

