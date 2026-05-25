# Study Plan: LangChain and LlamaIndex Agent Frameworks

**Audience**: senior-swe
**Depth**: working-knowledge
**Timeline**: 1mo (~5 hrs/week)
**Generated**: 2026-05-25

## How to use this plan

1. Start with `PLAN.md` for the phased milestones.
2. Use `curriculum.md` as the concept reference for each phase.
3. Use `resources.md` as the canon. Official docs come first because these frameworks change quickly.
4. Use `exercises.md` for the actual build work and design drills.
5. Update `progress.md` at the end of each week. Track surprise, not completion.
6. Re-open `interactive-plan.md` with an agent each week to run check-ins and adjust scope.

## Topic framing

The image topic is interpreted as "framework skill": building agent logic frameworks with the two mainstream Python ecosystems, LangChain/LangGraph and LlamaIndex.

This plan does not try to memorize every integration. The goal is to learn the reusable engineering model:

- tool schemas and model-tool binding
- agent loops vs deterministic workflows
- state, memory, checkpoints, and handoffs
- retrieval as a tool
- tracing, evaluation, and production constraints
- framework selection based on control, data orientation, and operational risk

## Diagrams

- `diagrams/roadmap.mmd` and `diagrams/roadmap.svg` — learning path flowchart
- `diagrams/timeline.mmd` and `diagrams/timeline.svg` — week-by-week gantt
- `diagrams/concept-map.excalidraw` — concept relationships

## Pedagogy reminders

- Build the same small agent in both frameworks before picking favorites.
- Treat framework APIs as volatile; learn concepts and inspect current docs.
- Prefer explicit state and traces over magical agent behavior.
- A framework is only useful if it improves debuggability, not just demo speed.

