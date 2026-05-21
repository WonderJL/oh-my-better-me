# Interactive Plan: Git Worktree for Agent Multitasking

**Timeline**: 1 day · ~6 focused hours  
**Started**: 2026-05-21  
**Last touched**: 2026-05-21

## How to run a session

1. Open this file when you sit down for the next block.
2. Find the next section with `Status: [ ] not started`.
3. Paste that section to your agent with:
   - what you finished in the previous block
   - what surprised you
   - what felt unsafe or unclear
4. The agent runs the **Check-in prompts**, scores your deliverable, and applies the **Adjust-if** rules to later blocks if needed.
5. Update `Status` and append notes under `Notes`.

---

## Session 1 — Build the mental model

- **Status**: `[ ] not started`
- **Goal**: Create real worktrees and explain the main-vs-linked model.
- **Pre-read** (≤30 min): Official `git worktree` docs: Description + Details.
- **Build / drill** (~45 min): Create one branch-backed and one detached worktree; inspect `git worktree list --porcelain` and each linked worktree's `.git` file.
- **Deliverable**: A short note explaining shared vs per-worktree state.
- **Check-in prompts**:
  1. Show the commands you ran and the `git worktree list` output.
  2. What is shared between the worktrees?
  3. What is separate per worktree?
  4. Why is this not the same as cloning?
- **Adjust-if**:
  - If you cannot explain `.git` in a linked worktree, re-read the Details section before moving on.
  - If the commands were trivial, add `git rev-parse --git-path HEAD` and inspect the result.
- **Notes**:

---

## Session 2 — Learn the guardrails

- **Status**: `[ ] not started`
- **Goal**: Understand branch checkout constraints and cleanup.
- **Pre-read** (≤30 min): `git worktree add`, `remove`, `prune`, `repair` sections.
- **Build / drill** (~90 min): Try same-branch checkout, remove clean worktrees, and write a safety checklist.
- **Deliverable**: A 6-bullet agent safety checklist.
- **Check-in prompts**:
  1. What happened when you tried to reuse a checked-out branch?
  2. When should an agent use `--detach`?
  3. What must be true before `git worktree remove` succeeds cleanly?
  4. What would make `--force` dangerous here?
- **Adjust-if**:
  - If branch constraints are unclear, repeat with a toy repo and draw the branch/worktree mapping.
  - If cleanup failed, inspect `git status` inside that worktree before doing anything else.
- **Notes**:

---

## Session 3 — Simulate multi-agent work

- **Status**: `[ ] not started`
- **Goal**: Assign independent tasks to separate worktrees without overlap.
- **Pre-read** (≤30 min): `git worktree list --porcelain`; skim `git config --worktree`.
- **Build / drill** (~90 min): Create three agent worktrees, make non-overlapping scratch changes, and inspect diffs from a coordinator worktree.
- **Deliverable**: A reusable agent dispatch prompt template.
- **Check-in prompts**:
  1. What absolute path, branch, and scope did each simulated agent get?
  2. Which files/resources were forbidden overlap?
  3. Which resources are still shared outside Git?
  4. How would you integrate the branches safely?
- **Adjust-if**:
  - If two tasks overlap, redesign the split before continuing.
  - If shared ports/caches/env vars are unclear, audit your actual project before using real agents.
- **Notes**:

---

## Session 4 — Write the playbook

- **Status**: `[ ] not started`
- **Goal**: Produce a personal playbook for worktree-based agent multitasking.
- **Pre-read** (≤15 min): Revisit only the docs sections you struggled with.
- **Build / drill** (~45 min): Write `worktree-agent-playbook.md` in your notes or scratch area.
- **Deliverable**: One-page playbook with commands, conventions, safety rules, and cleanup.
- **Check-in prompts**:
  1. Recite the create/list/remove commands from memory.
  2. Explain how you would split one feature across three agents.
  3. What is your cleanup sequence?
  4. What failure mode will you watch for first in real usage?
- **Adjust-if**:
  - If you cannot teach it back, shrink the playbook to the five commands you actually need and repeat the hands-on flow.
  - If you finished early, test `git worktree move` and read about `repair`.
- **Notes**:
