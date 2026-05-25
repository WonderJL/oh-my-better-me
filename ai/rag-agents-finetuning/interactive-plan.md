# Interactive Plan: RAG, AI Agents, and Model Fine-Tuning

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

## Phase 1: Retrieval Before Generation (Week 1)

### Week 1 — RAG baseline and retrieval traces

- **Status**: `[ ] not started`
- **Goal**: Build a minimal RAG system and expose retrieval quality before judging answer quality.
- **Pre-read** (<=2 hrs):
  - Lewis et al., "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks".
  - LlamaIndex "Introduction to RAG".
  - RAGAS paper sections on context relevance and faithfulness.
- **Build / drill** (~3 hrs):
  - Ingest 20-50 docs, chunk them, embed them, and answer questions with citations.
  - Log retrieved chunks, scores, answer, citations, latency, and cost.
  - Write 20 eval questions with expected evidence chunks.
- **Deliverable**: A working local RAG baseline plus `evals/questions.jsonl` or equivalent.
- **Check-in prompts**:
  1. Show the worst answer and its retrieved chunks. Was the failure retrieval or generation?
  2. Which chunking choice felt arbitrary, and how would you test it?
  3. What surprised you about the relationship between similarity scores and useful evidence?
  4. Write the 1-page explainer: why RAG is not just "put docs in the prompt".
- **Adjust-if**:
  - If the deliverable is incomplete, keep only 10 docs and 10 eval questions, then repeat this week.
  - If retrieval is strong but answers are weak, start Week 2 with stricter answer grounding before adding tools.
  - If this was trivial, add hybrid retrieval or reranking before moving to Week 2.
- **Notes**:

---

## Phase 2: From Pipeline to Agent (Week 2)

### Week 2 — Tool use with bounded control flow

- **Status**: `[ ] not started`
- **Goal**: Convert the RAG pipeline into a controlled tool-using assistant.
- **Pre-read** (<=2 hrs):
  - ReAct.
  - Toolformer.
  - OpenAI Agents guide or LangGraph agent overview.
- **Build / drill** (~3 hrs):
  - Wrap retrieval as `search_docs(query)` and source lookup as `fetch_source(id)`.
  - Add one deterministic tool, such as calculator, issue lookup, or structured metadata search.
  - Store each run as a structured trace with step count, tool calls, observations, and final answer.
  - Add max steps and a refusal path for unsupported answers.
- **Deliverable**: A bounded agent that can inspect docs, call one deterministic tool, and stop cleanly.
- **Check-in prompts**:
  1. Show one trace where the agent made a good tool choice and one where it wasted a step.
  2. What state did you need to make explicit that was previously hidden in the prompt?
  3. Which tool would be dangerous if exposed without approval?
  4. Write the 1-page explainer: why agents need state, limits, and traces.
- **Adjust-if**:
  - If the agent loops or overuses tools, reduce it to a fixed state machine with one optional tool call.
  - If tool choice is reliable, add one recovery path for failed retrieval.
  - If trace review is hard, postpone new tools and improve trace readability first.
- **Notes**:

---

## Phase 3: Evaluation and Production Boundaries (Week 3)

### Week 3 — Evals for the whole stack

- **Status**: `[ ] not started`
- **Goal**: Build an eval harness that compares prompt-only, RAG, and agentic RAG.
- **Pre-read** (<=2 hrs):
  - RAGAS paper.
  - DSPy paper or docs.
  - OpenAI practical guide to building agents.
- **Build / drill** (~3 hrs):
  - Run the same eval set against prompt-only, RAG, and agentic RAG.
  - Measure retrieval support, citation support, answer accuracy, latency, and cost.
  - Add at least five adversarial or ambiguous queries.
  - Write a decision table for prompt vs RAG vs agent vs fine-tune.
- **Deliverable**: Eval report with metrics, representative traces, and a decision table.
- **Check-in prompts**:
  1. Which metric changed your mind about the system?
  2. Which failure looked like a model issue but was actually an application issue?
  3. What would you block from release based on the current evals?
  4. Write the 1-page explainer: why evals must inspect intermediate artifacts.
- **Adjust-if**:
  - If metrics are too subjective, replace one LLM judge with a deterministic citation or retrieval assertion.
  - If all approaches look similar, improve the eval set before changing the system.
  - If agentic RAG is slower without quality gain, freeze agent work and focus on retrieval or prompting.
- **Notes**:

---

## Phase 4: Fine-Tuning With Restraint (Week 4)

### Week 4 — Fine-tuning decision and adapter dry run

- **Status**: `[ ] not started`
- **Goal**: Decide whether fine-tuning is justified and design or run a small adapter experiment.
- **Pre-read** (<=2 hrs):
  - LoRA.
  - QLoRA.
  - Hugging Face PEFT LoRA docs and current hosted fine-tuning docs if relevant.
- **Build / drill** (~3 hrs):
  - Mine 100-300 examples from earlier failures or synthetic-but-reviewed scenarios.
  - Redact, deduplicate, split, and validate the dataset.
  - Write training configuration, acceptance evals, rollback plan, and promotion criteria.
  - Optionally train a small LoRA adapter if hardware and time permit.
- **Deliverable**: Fine-tuning decision memo plus dataset audit and optional adapter checkpoint.
- **Check-in prompts**:
  1. Which examples prove this is a behavior problem rather than a missing-context problem?
  2. What data should be excluded even if it improves eval scores?
  3. What is the rollback plan if the tuned model regresses?
  4. Write the 1-page explainer: why fine-tuning is not a knowledge database.
- **Adjust-if**:
  - If examples are weak or noisy, do not train; spend the week improving the data audit.
  - If hosted fine-tuning is unavailable or unsuitable, use PEFT locally or keep this as a training job spec.
  - If fine-tuning is unnecessary, invest the remaining time in retrieval or agent eval improvements.
- **Notes**:

---

## End-of-plan review

Once all weeks are `done` or intentionally `skipped`, run this session with the agent:

1. Walk through every phase deliverable. What is worth showing?
2. Re-attempt the Week 1 teach-back. Is it sharper now?
3. List open questions from the `Notes` sections.
4. Decide whether to deepen, apply this stack to a real project, or pivot to the next topic.

