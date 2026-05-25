# Exercises: RAG, AI Agents, and Model Fine-Tuning

## Build projects

### Phase 1 build — Minimal RAG with traces

**Goal**: Build a local document Q&A service that can explain its own evidence.
**Scope**:

- Pick a corpus: internal docs, public API docs, or 20-50 PDFs/Markdown files.
- Parse, chunk, embed, and index documents.
- Answer questions with citations to chunk IDs and source paths.
- Log query, retrieved chunks, scores, answer, citations, latency, and cost.
- Create a 20-question eval set with expected evidence, not just expected answers.

**Stretch**: add hybrid retrieval and a reranker; compare recall@5 and answer faithfulness.

### Phase 2 build — Controlled agentic RAG

**Goal**: Turn retrieval into one tool among several while preserving debuggability.
**Scope**:

- Expose `search_docs(query)`, `fetch_source(id)`, and one deterministic tool.
- Give the agent a max-step limit and explicit stop conditions.
- Store traces as structured JSON.
- Add a "no answer if unsupported" branch.
- Create five adversarial prompts that try to force unsupported claims or tool misuse.

**Stretch**: add a human approval gate for any write or external-side-effect tool.

### Phase 3 build — Evals and decision boundaries

**Goal**: Prove which lever improves which failure mode.
**Scope**:

- Compare prompt-only, RAG, and agentic RAG on the same question set.
- Measure context recall, citation support, answer accuracy, latency, and cost.
- Add a trace grader for tool choice and unnecessary steps.
- Write a decision table: prompt, retrieve, tool, fine-tune, or deterministic code.

**Stretch**: run a DSPy-style optimization experiment against one pipeline component.

### Phase 4 build — Fine-tuning dry run or adapter experiment

**Goal**: Learn the fine-tuning workflow without pretending it is always the answer.
**Scope**:

- Mine failures from earlier phases.
- Convert 100-300 examples into the target training format.
- Deduplicate, redact, split train/validation, and write acceptance evals.
- If local hardware permits, train a tiny LoRA adapter on an open model.
- If not training, write the exact training job spec and promotion criteria.

**Stretch**: compare adapter output against RAG-only output on held-out failures.

## Design drills

- Design a RAG system for legal policy docs where citations are mandatory and wrong answers are worse than no answer.
- Design an agent that can inspect GitHub issues but cannot mutate anything without approval.
- Design a vector index migration plan when the embedding model changes.
- Design a tool permission model with read-only, write, external-side-effect, and secret-access tiers.
- Design a fine-tuning data pipeline where examples can be traced back to user consent and data retention policy.
- Decide whether a support bot failure should be fixed with prompt changes, retrieval, a tool, deterministic code, or fine-tuning.

## Read-and-explain

- Read the RAG paper's formulation of parametric vs non-parametric memory. Explain why that distinction still matters for application architecture.
- Read ReAct's examples. Explain which part is reasoning, which part is acting, and which part would need guardrails in production.
- Read Hugging Face PEFT's LoRA configuration surface. Explain what rank, target modules, alpha, and dropout change operationally.
- Read a framework's agent trace format. Explain what you would need to debug a bad tool call one week after deployment.
- Read a RAG eval library's metrics. Explain which metrics catch retrieval failure and which catch generation failure.

## Teach-back targets

By the end of each phase, write a 1-page explainer for:

- Phase 1: why RAG is not just "put docs in the prompt".
- Phase 2: why tool-using agents need explicit state, limits, and traces.
- Phase 3: why evals must inspect intermediate artifacts, not only final answers.
- Phase 4: why fine-tuning is behavior shaping, not a knowledge database.

