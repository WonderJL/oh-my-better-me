# Plan: LangChain and LlamaIndex Agent Frameworks

## Success criteria

By the end of the month, you should have:

- A small agent implemented twice: once with LangChain/LangGraph, once with LlamaIndex.
- A clear mental model for LangChain `create_agent`, LangGraph stateful orchestration, LlamaIndex `FunctionAgent`, `AgentWorkflow`, and LlamaIndex Workflows.
- A shared tool interface that can be adapted to both ecosystems.
- Trace logs and eval cases that expose tool choice, state updates, retrieval quality, and failure handling.
- A framework selection memo for at least three production scenarios.

## Phase 1: Same Agent, Two Frameworks (week 1)

**Goal**: Build the same minimal tool-using agent in LangChain and LlamaIndex.
**Deliverable**: Two working implementations of a support-triage agent with the same tools and the same eval questions.

- Build: implement `search_docs`, `lookup_ticket`, and `summarize_case` tools; run them through LangChain `create_agent` and LlamaIndex `FunctionAgent`.
- Read: LangChain agents docs; LlamaIndex "Building an agent"; current install and quickstart docs for both.
- Drill: define a framework-neutral tool contract with input schema, output schema, errors, and side-effect policy.
- Exit criteria: you can show how each framework represents tools, model calls, messages, and final output.

## Phase 2: Agent Loop vs Workflow Control (week 2)

**Goal**: Learn when to use an autonomous loop and when to make control flow explicit.
**Deliverable**: One LangGraph workflow and one LlamaIndex Workflow that implement the same triage process with explicit state transitions.

- Build: create a deterministic triage workflow: classify request, retrieve context, call the right tool, ask for human approval if needed, and emit a final response.
- Read: LangGraph workflows/agents and persistence docs; LlamaIndex Workflows and multi-agent patterns docs.
- Drill: draw the state machine and identify which edges should never be LLM-decided.
- Exit criteria: you can explain the difference between agent framework, agent runtime, and event-driven workflow.

## Phase 3: Retrieval, Memory, Observability, and Evals (week 3)

**Goal**: Add production surfaces that make agents debuggable and comparable.
**Deliverable**: A traced eval harness that runs both implementations against the same multi-turn cases.

- Build: add retrieval as a tool, session memory, structured traces, and 20 eval cases covering normal, ambiguous, adversarial, and tool-failure paths.
- Read: LangSmith observability concepts; LlamaIndex instrumentation/observability docs; framework eval examples.
- Drill: design a trace schema that survives framework migration.
- Exit criteria: you can compare LangChain/LangGraph and LlamaIndex on debuggability, ergonomics, retrieval fit, and operational risk.

## Phase 4: Production Selection and Hardening (week 4)

**Goal**: Turn framework knowledge into an engineering decision.
**Deliverable**: A framework selection memo plus a hardened reference implementation for one chosen scenario.

- Build: add timeouts, retry policy, max tool calls, prompt-injection checks for tool outputs, human approval for side effects, and cost/latency logging.
- Read: current production/deployment docs for LangGraph/LangSmith and LlamaIndex/LlamaCloud or deployment options.
- Drill: choose between LangChain, LangGraph, LlamaIndex, or plain Python for three real tasks.
- Exit criteria: you can defend a framework choice using requirements, not popularity.

## Open questions

- Does your target app need document-heavy retrieval more than flexible workflow orchestration?
- Are long-running tasks, checkpointing, and human approval first-class requirements?
- Do you need framework-native observability, or can you keep traces in a neutral schema?
- Which parts should be deterministic Python rather than delegated to an agent loop?
- What migration path exists if the selected framework's API changes?

