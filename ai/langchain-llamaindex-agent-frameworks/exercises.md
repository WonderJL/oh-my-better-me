# Exercises: LangChain and LlamaIndex Agent Frameworks

## Build projects

### Phase 1 build — Same agent in two frameworks

**Goal**: Learn framework shape by implementing the same behavior twice.
**Scope**:

- Define a support-triage task with three tools: `search_docs`, `lookup_ticket`, and `summarize_case`.
- Implement the tool schemas in plain Python first.
- Implement one version using LangChain `create_agent`.
- Implement one version using LlamaIndex `FunctionAgent`.
- Run both against the same 10 test prompts and compare traces.

**Stretch**: add structured final output with `category`, `confidence`, `citations`, and `next_action`.

### Phase 2 build — Explicit workflow control

**Goal**: Replace implicit agent looping with explicit state transitions.
**Scope**:

- Implement the same triage process in LangGraph.
- Implement the same triage process using LlamaIndex Workflows.
- Add states or events for classify, retrieve, tool_call, approval, final_response, and error.
- Add one human approval interrupt for side-effectful actions.

**Stretch**: persist state and resume after a simulated process restart.

### Phase 3 build — Retrieval, memory, traces, and evals

**Goal**: Make both versions measurable.
**Scope**:

- Add a small document corpus and expose retrieval as a tool.
- Add session memory for follow-up questions.
- Store traces in a neutral JSON format, independent of the framework.
- Create 20 eval cases: happy path, missing context, ambiguous request, malicious tool-output prompt, failed tool, and multi-turn correction.
- Compare both frameworks by outcome quality, trace clarity, latency, and implementation complexity.

**Stretch**: pipe traces into LangSmith or OpenTelemetry-compatible tooling.

### Phase 4 build — Harden one reference implementation

**Goal**: Select one framework for a scenario and harden it.
**Scope**:

- Pick one scenario: support triage, research report generation, document-review assistant, or workflow automation.
- Add timeouts, retries, max tool calls, structured errors, and cost/latency logging.
- Add prompt-injection checks for retrieved/tool output.
- Add approval gates for write or external-side-effect tools.
- Write a framework selection memo with decision matrix.

**Stretch**: package the chosen implementation behind a small HTTP API.

## Design drills

- Choose LangChain, LangGraph, LlamaIndex, or plain Python for a customer-support bot with strict citations.
- Choose a framework for a long-running research agent that may need to pause overnight and resume.
- Design a framework-neutral tool schema and trace schema that survive migration.
- Decide which workflow edges should be hard-coded and which can be model-decided.
- Design an observability plan that can explain a bad answer one week after deployment.
- Design a migration plan from a prototype LangChain agent to explicit LangGraph control.

## Read-and-explain

- Read LangChain's distinction between frameworks, runtimes, and harnesses. Explain where `create_agent` stops being enough.
- Read LangGraph's workflow/agent examples. Explain why state shape matters more than node count.
- Read LlamaIndex's FunctionAgent tutorial. Explain how tool outputs become context for the next model call.
- Read LlamaIndex's multi-agent patterns. Explain the tradeoff between `AgentWorkflow`, orchestrator, and custom planner.
- Read observability docs for both ecosystems. Explain what you would log in a neutral trace schema.

## Teach-back targets

By the end of each phase, write a 1-page explainer for:

- Phase 1: why decorators do not define an agent framework; tool contracts do.
- Phase 2: when to replace an agent loop with explicit workflow state.
- Phase 3: why observability and evals should be framework-independent.
- Phase 4: how to choose between LangChain/LangGraph, LlamaIndex, and plain Python.

