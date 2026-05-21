# Curriculum: Git Worktree for Agent Multitasking

## Tier 1 — Prerequisites

### Branches and refs
- **Definition**: Branches are movable refs under `refs/heads/*`; checking one out updates `HEAD` and the index for a worktree.
- **Leverage**: Worktree safety depends on knowing that Git protects a branch from being checked out in multiple worktrees by default.
- **Source**: https://git-scm.com/docs/git-branch, https://git-scm.com/docs/git-checkout
- **Maps onto**: Normal feature-branch workflow, but with multiple directories active at once.

### Clean working tree hygiene
- **Definition**: A worktree has its own tracked/untracked file state and index.
- **Leverage**: `git worktree remove` expects a clean worktree unless forced; agents should leave reviewable diffs.
- **Source**: https://git-scm.com/docs/git-worktree
- **Maps onto**: CI cleanup and release-branch hygiene.

## Tier 2 — Core concepts

### Main vs linked worktree
- **Definition**: A repository has one main worktree and zero or more linked worktrees attached to the same repository.
- **Leverage**: Enables parallel branches without clone sprawl.
- **Source**: https://git-scm.com/docs/git-worktree
- **Maps onto**: Multiple checkouts sharing one object database.

### Shared vs per-worktree state
- **Definition**: Objects and most refs are shared; `HEAD`, index, and some administrative files are per-worktree.
- **Leverage**: Explains what agents isolate and what they do not.
- **Source**: https://git-scm.com/docs/git-worktree, https://git-scm.com/docs/gitrepository-layout
- **Maps onto**: Process isolation where filesystem state differs but backend storage is shared.

### Branch-backed vs detached worktrees
- **Definition**: `git worktree add -b <branch> <path>` creates a branch-backed worktree; `--detach` creates scratch space at a commit.
- **Leverage**: Branch-backed is for deliverable work; detached is for experiments, tests, or read-only exploration.
- **Source**: https://git-scm.com/docs/git-worktree
- **Maps onto**: Persistent feature branch vs temporary sandbox.

### Worktree lifecycle commands
- **Definition**: `add`, `list`, `remove`, `prune`, `repair`, `move`, `lock`, `unlock` manage linked worktrees.
- **Leverage**: Prevents orphaned metadata and broken checkouts.
- **Source**: https://git-scm.com/docs/git-worktree
- **Maps onto**: Resource lifecycle management: create, inspect, cleanup, repair.

## Tier 3 — Advanced applications

### Worktree-specific config
- **Definition**: `extensions.worktreeConfig` enables per-worktree config via `git config --worktree`.
- **Leverage**: Useful when agents need different hooks, sparse-checkout, or local settings.
- **Source**: https://git-scm.com/docs/git-config, https://git-scm.com/docs/git-worktree
- **Maps onto**: Per-environment configuration overlays.

### Agent dispatch boundaries
- **Definition**: Each agent receives a unique absolute path, branch, scope, and forbidden-overlap list.
- **Leverage**: Prevents duplicated edits and integration chaos.
- **Source**: Operational pattern derived from Git worktree constraints.
- **Maps onto**: Sharding work across workers with explicit ownership.

### Integration strategy
- **Definition**: Compare branches, review diffs, merge or cherry-pick in a controlled order.
- **Leverage**: Multi-agent output is only useful if it can be reconciled cleanly.
- **Source**: https://git-scm.com/docs/git-diff, https://git-scm.com/docs/git-range-diff, https://git-scm.com/docs/git-merge
- **Maps onto**: Code review queues and stacked branches.

## Tier 4 — Frontier / follow-up

### Tool-managed worktrees
- **Definition**: AI coding tools may create isolated worktrees automatically for tasks or PR workflows.
- **Leverage**: You still need to understand the Git substrate to debug failures.
- **Source**: Tool-specific docs; verify for your agent host.
- **Maps onto**: Kubernetes-managed pods still requiring Linux process knowledge.

### Shared-state hardening
- **Definition**: Worktrees do not isolate databases, ports, caches, credentials, package stores, or background processes.
- **Leverage**: This is the main failure mode when scaling from two agents to many.
- **Source**: Project-specific audit.
- **Maps onto**: Test parallelization flakiness.

## Text concept map

`Repository object database` feeds many `worktrees`. Each worktree has its own `path`, `HEAD`, `index`, and working files. Each agent should own exactly one `path + branch + task scope`. Integration happens by comparing branches back in a coordinator worktree. Cleanup returns through `git worktree remove`, with `prune` and `repair` reserved for stale or moved worktrees.
