# Study Plan: LLM Internals

**Audience**: senior-swe
**Depth**: deep-mastery
**Timeline**: 1mo (~5 hrs/week)
**Generated**: 2026-05-20

You already know how to call an API. The goal here is to be able to (a) read any modern LLM paper without translating jargon, (b) sketch a transformer forward pass on a whiteboard from scratch, (c) reason about why training/inference behaves the way it does, and (d) modify a real model — not just prompt it.

## How to use this plan

1. Start with `PLAN.md` — 4 phases, one per week.
2. `curriculum.md` is the concept reference; consult per phase.
3. `resources.md` is the canon — primary sources (papers, repos) first.
4. `exercises.md` is what you'll actually build/do. Project-first: start coding day 2 of each week, not day 7.
5. `interactive-plan.md` is the operational weekly view — open it each session with the agent to drive check-ins and adjustments.
6. Update `progress.md` at end of each week. Track surprise, not completion.

## Diagrams
- `diagrams/roadmap.mmd` — learning path flowchart (Mermaid)
- `diagrams/timeline.mmd` — 4-week gantt (Mermaid)
- `diagrams/concept-map.excalidraw` — **skipped** (Excalidraw skill not invoked in this session); see the text concept map at the bottom of `curriculum.md` as a substitute.

Render Mermaid via `mmdc -i diagrams/roadmap.mmd -o diagrams/roadmap.svg` (or any Mermaid viewer).

## Pedagogy reminders

- **Project-first.** Each week starts coding by day 2. Friction reveals what's actually unknown.
- **Primary sources beat tutorials.** Papers, RFCs, source code (`nanoGPT`, `llama.cpp`, HF `transformers`). Tutorials only fill specific gaps.
- **Map new onto known.** Attention ≈ a learned soft dictionary lookup. KV cache ≈ memoization for an autoregressive recurrence. RLHF ≈ contextual bandits with a learned reward model.
- **Teach-back is the comprehension test.** End of each week, produce a 1-page explainer aimed at a hypothetical mid-level SWE.
- **Track surprise, not completion.** "What did I assume I knew but couldn't actually use?" is the only progress metric that matters.
