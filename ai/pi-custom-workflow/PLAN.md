# Plan: Pi Custom Workflow Architecture

## Phase 1: Foundation and Operating Model (Week 1)

**Goal**: Build a correct mental model of Pi as a terminal AI-coding harness rather than a fixed product.
**Deliverable**: A working Pi baseline plus a one-page explainer of its operating modes and customization surfaces.

- **Build**: Install/run Pi, perform three real coding-support tasks, and document which interactions should stay manual vs automated.
- **Read**: Official Pi docs at https://pi.dev/; installation/quickstart; interactive, print/JSON, RPC, and SDK concepts.
- **Drill**: Compare Pi to Claude Code/OpenCode/Codex as a harness: what is primitive, what is policy, what is extension?
- **Exit criteria**: You can explain sessions, tools, model/provider selection, and why Pi favors primitives over packed workflows.

## Phase 2: Workflow Primitives (Week 2)

**Goal**: Learn the building blocks the community uses to shape Pi: extensions, skills, prompt templates, packages, models, and themes.
**Deliverable**: A small personal workflow pack: one prompt template, one skill-style instruction bundle, and one written extension design.

- **Build**: Create a repeatable personal command for one real task, such as review-before-commit, debugging intake, or weekly planning.
- **Read**: Pi customization docs; extension/package concepts; model/provider configuration guidance.
- **Drill**: Decide which behaviors belong in a prompt template, skill, extension, SDK script, or external shell command.
- **Exit criteria**: You can justify each customization layer and avoid turning every preference into code.

## Phase 3: Architecture for Personal Workflows (Week 3)

**Goal**: Design a safe, composable Pi workflow architecture for your own development system.
**Deliverable**: A personal Pi architecture spec with routing rules, permissions, cost controls, and rollback strategy.

- **Build**: Prototype a workflow that uses Pi for one real loop: plan -> implement -> verify -> summarize, with explicit human approval gates.
- **Read**: Pi SDK/RPC docs; community examples and shared packages when available; source code around tool registration and lifecycle hooks if public.
- **Drill**: Threat-model your workflow: prompt injection, unsafe shell commands, accidental pushes, model cost blowups, and stale automation.
- **Exit criteria**: You can distinguish ergonomic automation from dangerous autonomy and describe where each guardrail lives.

## Phase 4: Economics, Safety, and Growth (Week 4)

**Goal**: Turn the prototype into an economical, maintainable operating system for your personal workflow.
**Deliverable**: A versioned personal workflow roadmap: current config, near-term improvements, budget policy, and community contribution plan.

- **Build**: Add measurement: model choice rules, token/cost notes, success/failure log, and a small regression checklist for your workflow.
- **Read**: Provider pricing pages for the models you actually use; Pi docs on model configuration; relevant community discussions.
- **Drill**: Decide what to share publicly, what to keep private, and how to review third-party Pi packages before installing them.
- **Exit criteria**: You can evolve Pi safely without copying random workflows blindly or creating hidden recurring cost.

## Open Questions

- Which Pi extension APIs are stable enough for long-lived personal automation?
- What is the healthiest community distribution channel: npm package, git repo, prompt template, or documentation pattern?
- Which parts of your current agent workflow are worth porting to Pi, and which should remain host-agnostic?
