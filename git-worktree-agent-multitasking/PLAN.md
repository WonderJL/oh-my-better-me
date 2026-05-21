# Plan: Git Worktree for Agent Multitasking

A one-day plan to move from "I know branches" to "I can safely coordinate multiple agents in separate worktrees and clean up afterward." The day is split into four sessions, each ending in a concrete artifact.

---

## Session 1: Mental model and first worktree (Hour 1)

**Goal**: Build the correct model: one repository object store, many working directories, separate `HEAD`/index per worktree.

**Deliverable**: Two linked worktrees created from a sandbox repo or disposable branch, with notes from `git worktree list --porcelain`.

- **Build**:
  - Run `git worktree list` in an existing repo.
  - Create a linked worktree: `git worktree add ../wt-demo-one -b wt-demo-one`.
  - Create a detached worktree: `git worktree add --detach ../wt-scratch HEAD`.
  - Inspect `.git` in each linked worktree; note that it is a file pointing back to the main repo metadata.
- **Read**:
  - `git worktree` official docs: description, commands, details.
- **Drill**:
  - Explain why this is cheaper than cloning.
  - Explain why it is more isolated than switching branches in one directory.
- **Exit criteria**:
  - You can define main worktree vs linked worktree.
  - You can say what is shared and what is per-worktree.
  - You can create a branch-backed and detached worktree without guessing.

---

## Session 2: Branch rules, safety, and cleanup (Hours 2-3)

**Goal**: Learn the constraints that prevent agents from stepping on each other.

**Deliverable**: A short safety checklist for agent assignment.

- **Build**:
  - Try to check out the same branch in two worktrees and observe Git refusing it.
  - Make a change in one worktree and verify the other worktree's files do not change.
  - Run `git worktree remove` on a clean worktree.
  - Simulate stale metadata only if you are using a disposable repo; otherwise read about `prune` and `repair` without forcing it.
- **Read**:
  - `git worktree add/remove/prune/repair` sections.
  - `git checkout` docs only for branch checkout constraints.
- **Drill**:
  - Decide when to use `-b`, existing branch, or `--detach`.
  - Write the rule: one agent, one branch, one worktree, one clear scope.
- **Exit criteria**:
  - You know why two agents should not share a branch.
  - You know the clean removal path.
  - You know when not to use `--force`.

---

## Session 3: Multi-agent operating model (Hours 4-5)

**Goal**: Convert worktree mechanics into an agent workflow.

**Deliverable**: A reusable multi-agent dispatch template.

- **Build**:
  - Create three task worktrees from the same base branch:
    - `../repo-agent-plan` for planning/docs
    - `../repo-agent-impl-a` for implementation slice A
    - `../repo-agent-impl-b` for implementation slice B
  - In each worktree, write a tiny marker file or make a trivial local-only change, then inspect status independently.
  - Practice merging or comparing outputs from the main worktree with `git diff <branch>` and `git range-diff` if branches diverge.
- **Read**:
  - `git worktree list --porcelain` output format.
  - `git config --worktree` and `extensions.worktreeConfig` overview.
- **Drill**:
  - Identify non-Git shared state: ports, databases, `.env`, caches, generated files, package manager stores, running dev servers.
  - Draft agent prompts that include absolute worktree paths and forbidden overlap.
- **Exit criteria**:
  - You can assign independent agents without path ambiguity.
  - You can inspect each agent's status without entering the wrong checkout.
  - You can name shared resources Git worktrees do not isolate.

---

## Session 4: Integration, teach-back, and personal playbook (Hour 6)

**Goal**: Finish with a playbook you can use on real agent work tomorrow.

**Deliverable**: `worktree-agent-playbook.md` in your notes or scratch area.

- **Build**:
  - Write a one-page playbook with commands for create/list/inspect/remove.
  - Include branch naming conventions and cleanup rules.
  - Include a merge/integration checklist.
- **Read**:
  - Revisit the official docs only for gaps discovered during hands-on work.
- **Drill**:
  - Teach back: "How I would split a feature across three agents using worktrees."
  - Failure drill: an agent says its worktree is broken after a manual move; explain how `repair` fits.
- **Exit criteria**:
  - You can create and remove worktrees safely.
  - You can design a multi-agent split with no branch/path ambiguity.
  - You know the cleanup and integration sequence.

---

## Open questions to revisit after today

- Should your default worktree location be sibling directories, `~/.worktrees/<repo>/`, or tool-managed worktree roots?
- How should your team name branches for agent tasks?
- Which project-specific resources are still shared despite worktree isolation?
- Does your agent tooling support spawning directly inside a target worktree?
