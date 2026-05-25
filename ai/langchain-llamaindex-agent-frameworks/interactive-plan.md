# Interactive Plan: LangChain and LlamaIndex Agent Frameworks

**Timeline**: 4 weeks · ~5 hrs/week
**Started**: 2026-05-25
**Last touched**: 2026-05-25

## How to run a week

1. Open this file when you sit down for the week's session.
2. Find the next week with `Status: [ ] not started`.
3. Paste that week's section to your agent along with what you finished, what surprised you, and what blocked you.
4. The agent runs the Check-in prompts, scores your deliverable, and applies the Adjust-if rules to later weeks if needed.
5. Update `Status` and append notes under `Notes`.

---

## Phase 1: Same Agent, Two Frameworks (Week 1)

### Week 1 — Build the comparison baseline

- **Status**: `[ ] not started`
- **Goal**: Implement the same minimal support-triage agent in LangChain and LlamaIndex.
- **Pre-read** (<=2 hrs):
  - LangChain agents docs.
  - LlamaIndex "Building an agent".
  - LangChain frameworks/runtimes overview.
- **Build / drill** (~3 hrs):
  - Define three plain Python tools: `search_docs`, `lookup_ticket`, and `summarize_case`.
  - Wrap them with LangChain `create_agent`.
  - Wrap them with LlamaIndex `FunctionAgent`.
  - Run both against the same 10 prompts.
- **Deliverable**: Two implementations plus a comparison table of tool schema, trace shape, ergonomics, and failure modes.
- **Check-in prompts**:
  1. Show the same prompt running in both frameworks. Where does the code differ in meaningful ways?
  2. Which framework hid useful details from you?
  3. Which tool contract detail was awkward to express?
  4. Write the 1-page explainer: why tool contracts matter more than decorators.
- **Adjust-if**:
  - If setup dominates the week, reduce to one tool and three prompts, then finish the comparison.
  - If both implementations are trivial, add structured output and one bad-tool-response case.
  - If one framework blocks progress, document the blocker and continue with the other rather than losing the week.
- **Notes**:

---

## Phase 2: Agent Loop vs Workflow Control (Week 2)

### Week 2 — Make state explicit

- **Status**: `[ ] not started`
- **Goal**: Rebuild the triage process as explicit workflow control in both ecosystems.
- **Pre-read** (<=2 hrs):
  - LangGraph workflows and agents docs.
  - LangGraph persistence docs.
  - LlamaIndex Workflows and multi-agent patterns docs.
- **Build / drill** (~3 hrs):
  - Create a LangGraph workflow with classify, retrieve, approve, respond, and error states.
  - Create an equivalent LlamaIndex Workflow with typed events and steps.
  - Add one human approval gate for side-effectful actions.
  - Simulate one failure and one retry path.
- **Deliverable**: Two explicit workflow implementations and a state/event diagram.
- **Check-in prompts**:
  1. Which edges should be deterministic and which can remain model-decided?
  2. What state must persist across turns or crashes?
  3. Which workflow was easier to reason about without running it?
  4. Write the 1-page explainer: when to replace an agent loop with workflow state.
- **Adjust-if**:
  - If both workflows become too large, implement only classify -> retrieve -> respond, then add approval later.
  - If one framework's workflow model is much clearer, use it as the reference and write an adapter plan for the other.
  - If the approval gate is hard, replace it with a no-op interrupt and document the intended production behavior.
- **Notes**:

---

## Phase 3: Retrieval, Memory, Observability, and Evals (Week 3)

### Week 3 — Make behavior measurable

- **Status**: `[ ] not started`
- **Goal**: Add retrieval, memory, traces, and evals that compare both implementations.
- **Pre-read** (<=2 hrs):
  - LangSmith observability concepts.
  - LlamaIndex instrumentation or observability docs.
  - Framework eval examples from official docs or repos.
- **Build / drill** (~3 hrs):
  - Add retrieval as a tool backed by a small document corpus.
  - Add session memory for follow-up prompts.
  - Emit a neutral JSON trace for each run.
  - Write 20 eval cases covering normal, ambiguous, malicious, and failed-tool paths.
- **Deliverable**: Eval report comparing outcome quality, trace clarity, latency, and complexity.
- **Check-in prompts**:
  1. Which framework made the bad answer easiest to debug?
  2. Which trace fields should exist regardless of framework?
  3. What failure did your eval set miss until you saw a real trace?
  4. Write the 1-page explainer: why observability and evals should be framework-independent.
- **Adjust-if**:
  - If evals take too long, keep five high-signal cases and one deterministic assertion per case.
  - If retrieval dominates the work, reduce the corpus and keep the framework comparison moving.
  - If traces are noisy, define a canonical trace schema before adding more cases.
- **Notes**:

---

## Phase 4: Production Selection and Hardening (Week 4)

### Week 4 — Choose and harden

- **Status**: `[ ] not started`
- **Goal**: Pick the right framework for a concrete scenario and harden a reference implementation.
- **Pre-read** (<=2 hrs):
  - Current LangGraph or LangSmith production/deployment docs.
  - Current LlamaIndex deployment, LlamaCloud, or observability docs.
  - Release notes or migration notes for whichever framework you plan to use.
- **Build / drill** (~3 hrs):
  - Choose one scenario and one framework.
  - Add timeouts, retries, max tool calls, structured errors, and cost/latency logs.
  - Add prompt-injection checks for retrieved or tool output.
  - Add an approval gate for write/external-side-effect tools.
  - Write a framework selection memo.
- **Deliverable**: Hardened reference implementation plus selection memo.
- **Check-in prompts**:
  1. What requirement decided the framework choice?
  2. Which part should be plain Python even inside the framework?
  3. What would make you migrate away from this choice?
  4. Write the 1-page explainer: how to choose between LangChain/LangGraph, LlamaIndex, and plain Python.
- **Adjust-if**:
  - If the hardening work is too broad, prioritize max tool calls, timeout, and trace logging.
  - If no framework clearly wins, choose based on the next real project and record the risk.
  - If deployment docs are stale or unclear, keep the implementation local and write deployment unknowns explicitly.
- **Notes**:

---

## End-of-plan review

Once all weeks are `done` or intentionally `skipped`, run this session with the agent:

1. Walk through every phase deliverable. What is worth showing?
2. Re-attempt the Week 1 teach-back. Is it sharper now?
3. List open questions from the `Notes` sections.
4. Decide whether to deepen on LangGraph, deepen on LlamaIndex, or apply the comparison to a real project.

