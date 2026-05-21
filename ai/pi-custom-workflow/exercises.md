# Exercises: Pi Custom Workflow Architecture

## Build Projects

### Phase 1 Build — Baseline Operator Notebook

**Goal**: Use Pi for real tasks while recording what the harness actually does.
**Scope**: Run three sessions: one explanation task, one code-change task in a disposable repo, and one review/debugging task.
**Deliverable**: `operator-notes.md` with commands used, model/provider choice, surprising behavior, and manual approval points.
**Stretch**: Repeat one task with a cheaper or local model and compare quality/cost.

### Phase 2 Build — Personal Workflow Pack v0

**Goal**: Encode one repeatable workflow without over-engineering it.
**Scope**: Create one prompt template, one skill-style instruction bundle, and one extension design document. Implement only the lowest-risk layer first.
**Deliverable**: A versioned folder containing the template/instructions plus `why-this-layer.md`.
**Stretch**: Convert the design into a real extension only after the prompt/template version proves valuable.

### Phase 3 Build — Safe Workflow Architecture Prototype

**Goal**: Prototype a plan -> implement -> verify -> summarize loop with explicit guardrails.
**Scope**: Use a disposable repo. Define allowed tools, blocked actions, consent gates, model routing, and logging.
**Deliverable**: `pi-personal-architecture.md` plus a runnable or manually reproducible workflow demo.
**Stretch**: Add SDK or RPC integration for one step where scripting is clearly better than conversation.

### Phase 4 Build — Cost and Safety Hardening

**Goal**: Make the workflow economically and operationally sustainable.
**Scope**: Add model selection rules, cost notes, package review checklist, rollback process, and failure log.
**Deliverable**: `personal-pi-roadmap.md` with current state, next three improvements, and stop-doing list.
**Stretch**: Publish a sanitized workflow pattern or write a community post explaining the architecture tradeoffs.

## Design Drills

- Design a Pi workflow that can inspect code and propose commits but cannot push or create PRs without approval. Defend the boundary.
- Decide whether a recurring task belongs in a prompt template, skill, extension, package, SDK script, or shell alias.
- Create a model-routing table for trivial edits, debugging, architecture, code review, and long-running exploration.
- Threat-model installing a community Pi package. What do you inspect before running it?
- Design a rollback strategy for a broken self-modifying workflow.

## Read-and-Explain

- Read Pi docs for extensions and explain how a custom tool becomes available to the agent.
- Read Pi docs for model/provider configuration and explain how you would add or restrict providers.
- Read one community package or example workflow and explain the install path, permissions, and hidden assumptions.
- Read your own current agent workflow config and explain what should move to Pi, what should stay global, and why.

## Teach-Back Targets

By the end of each phase, write a one-page explainer for:

- Phase 1: "Pi as a harness, not a product."
- Phase 2: "The right customization layer for the job."
- Phase 3: "Safe personal workflow architecture in Pi."
- Phase 4: "Economical growth: when to automate, when to stop."
