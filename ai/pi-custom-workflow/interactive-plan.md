# Interactive Plan: Pi Custom Workflow Architecture

**Timeline**: 4 weeks · ~5 hrs/week
**Started**: 2026-05-22
**Last touched**: 2026-05-22

## How to run a week

1. Open this file when you sit down for the week's session.
2. Find the next week with `Status: [ ] not started`.
3. Paste that week's section to your agent with what you finished, what surprised you, and what blocked you.
4. The agent runs the check-in prompts, scores your deliverable, and applies the adjust-if rules.
5. Update `Status`, append notes under `Notes`, and commit if this folder is tracked.

---

## Phase 1: Foundation and Operating Model

### Week 1 — Baseline Operator Model

- **Status**: `[ ] not started`
- **Goal**: Understand Pi's operating modes and use it on real low-risk tasks.
- **Pre-read** (≤2 hrs):
  - Pi official docs at https://pi.dev/
  - Quickstart/install and usage-mode sections from the official docs
- **Build / drill** (~3 hrs):
  - Run three Pi sessions: explain, code in a disposable repo, and review/debug.
  - Write `operator-notes.md` with what Pi did, what you approved, and what felt risky.
- **Deliverable**: Working Pi baseline plus a one-page explainer: "Pi as a harness, not a product."
- **Check-in prompts**:
  1. Show me `operator-notes.md`. Which interaction should never be automated?
  2. Which Pi mode did you understand least: interactive, print/JSON, RPC, or SDK?
  3. What surprised you about model/provider behavior?
  4. Explain Pi to a senior engineer who already uses another coding agent.
- **Adjust-if**:
  - If setup or auth blocked you, repeat Week 1 with only install, one session, and model switching.
  - If the tasks felt trivial, add a fourth session using a cheaper/local model and compare output.
  - If the explainer is vague, spend one extra hour mapping Pi concepts to shell/editor/plugin analogues.
- **Notes**:

---

## Phase 2: Workflow Primitives

### Week 2 — Customization Layers

- **Status**: `[ ] not started`
- **Goal**: Decide which Pi customization layer fits which workflow need.
- **Pre-read** (≤2 hrs):
  - Pi docs for extensions, skills, prompt templates, packages, and model/provider config
  - One verified community example if available from official links or source search
- **Build / drill** (~3 hrs):
  - Create a personal workflow pack v0: one prompt template, one skill-style instruction bundle, and one extension design.
  - For each item, write why it belongs at that layer.
- **Deliverable**: `personal-workflow-pack-v0/` and `why-this-layer.md`.
- **Check-in prompts**:
  1. Which customization did you almost over-engineer?
  2. What belongs in an extension that should not live in a prompt template?
  3. Which community pattern looked useful but unsafe or too expensive?
  4. Teach back the difference between template, skill, extension, package, SDK, and RPC.
- **Adjust-if**:
  - If you did not build anything, reduce scope to one prompt template and one written extension decision.
  - If this felt easy, implement the lowest-risk customization and test it in a disposable repo.
  - If layer choice is unclear, create a decision table before Week 3.
- **Notes**:

---

## Phase 3: Architecture and Guardrails

### Week 3 — Personal Workflow Architecture

- **Status**: `[ ] not started`
- **Goal**: Design a safe plan -> implement -> verify -> summarize Pi workflow for your own work.
- **Pre-read** (≤2 hrs):
  - Pi SDK/RPC docs from https://pi.dev/
  - Pi docs or source around tools, permissions, lifecycle hooks, and providers
- **Build / drill** (~3 hrs):
  - Prototype the workflow in a disposable repo with explicit allowed tools, blocked actions, human consent gates, and model routing.
  - Write a threat model covering shell, git, secrets, package installs, prompts, and cost.
- **Deliverable**: `pi-personal-architecture.md` plus a reproducible workflow demo or transcript.
- **Check-in prompts**:
  1. Which action is most dangerous if Pi performs it without asking?
  2. Where does your workflow log enough state for debugging?
  3. Which guardrail is policy, which is configuration, and which requires code?
  4. Explain why your model routing is safe and economical.
- **Adjust-if**:
  - If the prototype is too broad, keep only one loop: review-before-commit.
  - If the guardrails are hand-wavy, pause implementation and write an explicit permission matrix.
  - If the prototype works well, add one SDK/RPC integration where scripting clearly helps.
- **Notes**:

---

## Phase 4: Economics and Community Growth

### Week 4 — Hardening and Evolution

- **Status**: `[ ] not started`
- **Goal**: Convert the prototype into a sustainable personal Pi operating model.
- **Pre-read** (≤2 hrs):
  - Provider pricing pages for the models you actually use
  - Official Pi docs for model/provider configuration and package/customization mechanisms
- **Build / drill** (~3 hrs):
  - Add cost notes, package review checklist, failure log, rollback process, and a next-steps roadmap.
  - Review one community workflow as if it were a production dependency.
- **Deliverable**: `personal-pi-roadmap.md` with current setup, next three improvements, stop-doing list, and sharing plan.
- **Check-in prompts**:
  1. What recurring cost could surprise you after a month?
  2. Which automation should you delete or keep manual?
  3. What would you share with the community, and what should stay private?
  4. Re-explain Week 1's harness model. What is sharper now?
- **Adjust-if**:
  - If cost policy is missing, do not add more automation until routing and budget rules exist.
  - If safety review is weak, repeat the community-package audit with a stricter checklist.
  - If everything is stable, pick one small contribution: docs note, template, package review, or architecture write-up.
- **Notes**:

---

## End-of-Plan Review

1. Walk through every phase deliverable. Which artifact would help future you most?
2. Re-attempt the Week 1 teach-back. Is it clearer and more concrete?
3. List unresolved questions from `Notes` and classify them as docs/source/community/experiment.
4. Decide the next move: deepen Pi internals, apply Pi to a real workflow, or extract host-agnostic patterns.
