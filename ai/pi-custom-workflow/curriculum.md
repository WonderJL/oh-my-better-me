# Curriculum: Pi Custom Workflow Architecture

## Tier 1 — Prerequisites

- ✅ Terminal fluency — shell, environment variables, paths, editors, and process control.
- ✅ Git safety — status/diff/log, branches, commits, and what not to automate without consent.
- ✅ LLM basics — context windows, tool use, system/developer/user instruction hierarchy, and prompt injection risk.
- 📖 Node.js and TypeScript module basics — enough to read or write Pi extensions.
- 📖 API/provider economics — model pricing, rate limits, latency, and when local models are viable.
- 🔬 Threat modeling for agentic tools — actually study if you have not designed permission gates before.

## Tier 2 — Core

### Pi as Harness

- **Definition**: Pi is best understood as a minimal terminal-based AI-coding harness that exposes primitives for workflows rather than a finished workflow product.
- **Leverage**: This prevents cargo-culting someone else's setup and keeps customization intentional.
- **Source**: Official Pi docs at https://pi.dev/.
- **Maps onto**: A Unix shell plus editor plus plugin host, not a SaaS dashboard.

### Operating Modes

- **Definition**: Interactive TUI, print/JSON automation, RPC integration, and SDK embedding are different ways to drive the same agent substrate.
- **Leverage**: Mode choice determines whether a workflow should be conversational, scripted, embedded, or integrated with another system.
- **Source**: Pi docs on usage modes.
- **Maps onto**: CLI interactive mode vs library API vs daemon protocol.

### Customization Layers

- **Definition**: Prompt templates, skills, extensions, packages, themes, and model/provider config are separate customization surfaces.
- **Leverage**: The right layer keeps the workflow simple; the wrong layer creates brittle automation.
- **Source**: Pi customization docs.
- **Maps onto**: Shell alias, Make target, editor plugin, npm package, and config file.

### Extension Lifecycle

- **Definition**: Extensions are TypeScript modules that hook into Pi's lifecycle to register tools, commands, UI behavior, providers, or policies.
- **Leverage**: Extensions are where powerful behavior lives, so they need stronger review and tests than prompt-only changes.
- **Source**: Pi extension docs and source code.
- **Maps onto**: VS Code extension activation plus command registration.

### Permission and Consent Gates

- **Definition**: A workflow-level policy for which actions can run automatically and which need explicit human approval.
- **Leverage**: The difference between useful autonomy and an agent that can damage repos, credentials, or bills.
- **Source**: Pi docs plus your own safety policy.
- **Maps onto**: Production change-management gates and least-privilege access control.

### Model Routing and Cost Policy

- **Definition**: Rules for selecting providers/models based on task difficulty, latency, privacy, and cost.
- **Leverage**: Most personal workflows fail by becoming either too expensive or too weak for hard tasks.
- **Source**: Pi model/provider docs and provider pricing pages.
- **Maps onto**: SLO-based service tiering.

### Community Workflow Evaluation

- **Definition**: A review process for packages, templates, skills, and extensions shared by other users.
- **Leverage**: Community velocity is valuable only if you can safely inspect and adapt what you import.
- **Source**: Community repos/discussions plus package source.
- **Maps onto**: Dependency review before adding a build plugin.

## Tier 3 — Advanced

- Designing workflows as small reversible layers: prompt template first, extension only when necessary.
- Building a personal command taxonomy: planning, implementation, review, debugging, summarization, maintenance.
- Composing Pi with tmux, git worktrees, issue trackers, and local scripts without hiding state.
- Sandboxing high-risk tools and limiting filesystem/network authority.
- Versioning personal workflows so changes can be diffed, reviewed, and rolled back.
- Measuring workflow ROI: saved time, failure modes, token/model cost, and cognitive overhead.
- Auditing third-party Pi packages for prompt injection, shell access, secrets handling, and update risk.

## Tier 4 — Frontier

- Self-modifying agent harnesses that can safely edit their own extensions.
- Portable workflow packages across Pi, OpenCode, Claude Code, Codex, and other agent hosts.
- Typed agent-tool contracts and reproducible permission manifests.
- Local-first model routing for private or low-cost loops, escalating only hard tasks to frontier models.
- Community governance for shared agent workflow packages.
