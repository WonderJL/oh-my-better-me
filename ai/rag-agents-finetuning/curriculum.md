# Curriculum: RAG, AI Agents, and Model Fine-Tuning

## Tier 1 — Prerequisites

- ✅ Transformer inference basics — know tokens, context windows, attention, decoding, and prompt construction.
- ✅ API integration and service design — know retries, timeouts, idempotency, observability, and schema evolution.
- 📖 Embeddings and vector search — refresh cosine similarity, nearest-neighbor search, recall/precision, and metadata filters.
- 📖 Information retrieval basics — refresh BM25, dense retrieval, hybrid search, reranking, and query rewriting.
- 📖 ML training loop vocabulary — refresh dataset splits, loss, overfitting, validation, gradient updates, and checkpoints.
- 🔬 Evaluation design — actually study if rusty; this stack is impossible to improve without useful evals.

## Tier 2 — Core

### Retrieval-Augmented Generation

- **Definition**: A generation pipeline that retrieves external context at inference time and conditions the model on that context.
- **Leverage**: Lets applications use private, fresh, or large knowledge without changing model weights.
- **Source**: Lewis et al., "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks".
- **Maps onto**: Database-backed web apps, where the model is the view layer and retrieval is the query layer.

### Chunking and Index Design

- **Definition**: The process of turning raw documents into retrievable units with metadata and stable identifiers.
- **Leverage**: Bad chunking silently destroys retrieval quality before the model sees anything.
- **Source**: LlamaIndex RAG documentation; vector database docs for metadata filtering.
- **Maps onto**: Database schema design and indexing strategy.

### Retrieval Evaluation

- **Definition**: Measuring whether the right evidence appears before judging whether the answer is good.
- **Leverage**: Separates "the model hallucinated" from "the application gave the model the wrong context".
- **Source**: RAGAS paper and retrieval metrics such as recall@k, MRR, and nDCG.
- **Maps onto**: Integration tests with assertions at subsystem boundaries.

### Tool-Using Agents

- **Definition**: LLM-centered systems that choose among tools, observe results, and continue toward a goal under control-flow constraints.
- **Leverage**: Moves from single-turn answer generation to multi-step task completion.
- **Source**: ReAct and Toolformer.
- **Maps onto**: Workflow orchestration with a probabilistic planner inside the loop.

### Agent State and Traces

- **Definition**: Explicit representation of task state, tool calls, observations, decisions, and termination conditions.
- **Leverage**: Without traces, agent behavior is not debuggable or governable.
- **Source**: OpenAI Agents docs, LangGraph docs, and production tracing tools.
- **Maps onto**: Distributed tracing for services, except spans include model decisions and tool observations.

### Fine-Tuning

- **Definition**: Updating model behavior with curated examples, either by changing all weights or by training adapters.
- **Leverage**: Useful for durable style, format, classification, or domain behavior; poor fit for fast-changing facts.
- **Source**: LoRA, QLoRA, PEFT docs, and hosted SFT docs.
- **Maps onto**: Baking a frequently repeated transformation into a lower-level component after proving it is stable.

### Data Quality for Tuning

- **Definition**: Selecting, formatting, deduplicating, labeling, and validating examples used to modify behavior.
- **Leverage**: Fine-tuning quality is usually capped by dataset quality, not the training command.
- **Source**: Provider fine-tuning guides and PEFT examples.
- **Maps onto**: Building a production migration: input quality and rollback matter more than the command.

## Tier 3 — Advanced / Applied

- Hybrid retrieval: combine BM25, dense vectors, metadata filters, and rerankers.
- Query transformation: decomposition, HyDE-style expansion, multi-hop retrieval, and conversational query rewriting.
- Context packing: citation spans, deduplication, section ordering, and budget allocation.
- Agent control: state machines, human approval gates, tool schemas, idempotent actions, and bounded recursion.
- Agent safety: prompt injection, tool output poisoning, secrets exposure, external side effects, and audit trails.
- Evals: golden sets, adversarial sets, LLM-as-judge calibration, trace grading, latency and cost budgets.
- Fine-tuning methods: SFT, instruction tuning, DPO/preference tuning, prompt tuning, adapters, LoRA, QLoRA.
- Adapter operations: adapter composition, serving many adapters, rollback, versioning, and eval-gated promotion.
- Build-vs-buy: hosted vector stores, local vector DBs, OpenAI/Anthropic/Google agent frameworks, LangGraph, LlamaIndex, and DSPy.

## Tier 4 — Frontier

- Agentic RAG: agents that decide when and how to retrieve, rather than fixed retrieve-then-read pipelines.
- Self-reflective RAG: systems that critique retrieval sufficiency and answer support before finalizing.
- Memory systems: separating episodic memory, semantic memory, working context, and durable user preferences.
- Multimodal RAG: retrieval over images, PDFs, tables, audio, and video with structured extraction.
- Programmatic LM pipelines: DSPy-style optimization of prompts and weights from eval signals.
- Fine-tuning plus retrieval: deciding how adapters, vector stores, and tool policies co-evolve.
- Long-running agents: persistence, interrupts, resumability, observability, and governance.
- Post-training for reasoning/tool use: reinforcement learning, process supervision, and synthetic tool-use data.

