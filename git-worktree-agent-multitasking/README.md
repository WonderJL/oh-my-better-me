# Study Plan: Git Worktree for Agent Multitasking

**Audience**: senior-swe  
**Depth**: working-knowledge  
**Timeline**: 1 day (~6 focused hours)  
**Generated**: 2026-05-21

The goal is to understand `git worktree` well enough to confidently run multiple AI agents on independent tasks without branch collisions, hidden shared-state surprises, or cleanup debt.

## How to use this plan

1. Start with `PLAN.md` — the one-day schedule and exit criteria.
2. Use `curriculum.md` as the concept reference during breaks.
3. Use `resources.md` for official docs and follow-up reading.
4. Do the hands-on flow in `exercises.md`; this topic only sticks when you create, inspect, and remove real worktrees.
5. Use `interactive-plan.md` during the day with an agent: paste each session section back with your notes.
6. Update `progress.md` at lunch and at the end. Track surprise, not completion.

## Diagrams

- `diagrams/roadmap.mmd` — learning path flowchart (Mermaid)
- `diagrams/timeline.mmd` — one-day Gantt (Mermaid)
- `diagrams/concept-map.excalidraw` — **skipped**; see the text concept map in `curriculum.md`.

Render Mermaid via:

```bash
mmdc -i diagrams/roadmap.mmd -o diagrams/roadmap.svg
mmdc -i diagrams/timeline.mmd -o diagrams/timeline.svg
```

## Pedagogy reminders

- **Project-first.** You will create real worktrees in hour 1, not after reading everything.
- **Primary sources first.** Official Git docs drive the plan; blogs are only workflow color.
- **Map new onto known.** Worktrees are not clones. They are isolated working directories attached to one shared repository database.
- **Teach-back is the test.** End the day by explaining when to use worktrees, when not to, and how to safely assign agents.
- **Track surprise.** Record which assumptions from normal branch-based work were wrong.
