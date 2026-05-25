# Plan: RAG, AI Agents, and Model Fine-Tuning

## Success criteria

By the end of the month, you should have a small but defensible LLM application with:

- A document ingestion and retrieval pipeline with measurable retrieval quality.
- A grounded answer generator with citations and failure-mode logging.
- A controlled tool-using agent with explicit state, bounded tools, and trace review.
- A fine-tuning decision memo plus a small PEFT/SFT experiment plan, even if you choose not to train.
- A short eval suite that lets you compare RAG-only, agentic RAG, and fine-tuned behavior.

## Phase 1: Retrieval Before Generation (week 1)

**Goal**: Build the smallest useful RAG system and make retrieval failures visible.
**Deliverable**: A local "ask my docs" service over a small corpus, with retrieval traces and a 20-question eval set.

- Build: ingest 20-50 documents, chunk them, embed them, retrieve top-k passages, and generate answers with citations.
- Read: Lewis et al. 2020 RAG paper; LlamaIndex "Introduction to RAG"; RAGAS paper sections on context relevance and faithfulness.
- Drill: design a chunking strategy for API docs, policy docs, and code docs. Defend different chunk sizes and metadata choices.
- Exit criteria: you can explain three bad answers by pointing to retrieval, reranking, missing metadata, or generation behavior.

## Phase 2: From Pipeline to Agent (week 2)

**Goal**: Add tool use and control flow without turning the system into an unbounded autonomous loop.
**Deliverable**: A tool-using assistant that can search docs, inspect metadata, call one external deterministic tool, and stop cleanly.

- Build: wrap retrieval as a tool, add one deterministic tool such as a calculator, issue lookup, or filesystem index, and record a structured trace for every step.
- Read: ReAct; Toolformer; OpenAI Agents guide or LangGraph agent overview.
- Drill: design a permission model for tools with read-only, write, and external-side-effect tiers.
- Exit criteria: the agent can recover from one failed retrieval or tool call, and you can inspect why it chose each action.

## Phase 3: Evaluation and Production Boundaries (week 3)

**Goal**: Turn anecdotes into evals and define when RAG, agents, or fine-tuning is the right lever.
**Deliverable**: An eval harness that compares baseline prompting, RAG, and agentic RAG across answer quality, grounding, latency, and cost.

- Build: create a golden set, adversarial queries, citation checks, retrieval metrics, and a simple trace grader.
- Read: RAGAS; DSPy paper or docs for optimizing LM pipelines; OpenAI practical guide to building agents.
- Drill: design a rollback strategy for a production LLM app when the vector index, model, or tool schema changes.
- Exit criteria: you have a written decision table for prompt vs RAG vs agent vs fine-tune, backed by example failures.

## Phase 4: Fine-Tuning With Restraint (week 4)

**Goal**: Understand fine-tuning as a narrow behavior-shaping tool, not a replacement for memory, tools, or evals.
**Deliverable**: A fine-tuning decision memo and a small LoRA/QLoRA or hosted SFT experiment design using data produced by earlier phases.

- Build: prepare 100-300 high-quality examples from observed failures; run a dry-run data audit; optionally train a small LoRA adapter on an open model if hardware permits.
- Read: LoRA; QLoRA; Hugging Face PEFT LoRA docs; current OpenAI fine-tuning docs if using hosted models.
- Drill: compare full fine-tuning, LoRA, QLoRA, prompt optimization, and retrieval improvements for a domain-specific support assistant.
- Exit criteria: you can state exactly what behavior belongs in weights and what must remain in retrieved context or tools.

## Open questions

- Which failures in your domain are knowledge failures, reasoning failures, tool-access failures, or behavior/style failures?
- What is the cheapest eval that would have caught the last serious answer defect?
- Where do you need deterministic code rather than an agent step?
- Which data is legally and operationally safe to use for fine-tuning?
- What should be monitored after deployment: retrieval drift, tool errors, model changes, or user trust signals?

