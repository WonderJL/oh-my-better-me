# Exercises: Git Worktree for Agent Multitasking

## Exercise 1 — Create and inspect two worktrees

**Scope**: Use a disposable repository or a low-risk personal repo.

```bash
git worktree list
git worktree add ../wt-demo-one -b wt-demo-one
git worktree add --detach ../wt-scratch HEAD
git worktree list --porcelain
```

**Verify**:
- Each worktree appears in `git worktree list`.
- The linked worktree `.git` is a file, not a directory.
- `git status` differs independently in each path.

**Teach-back**: Why is a linked worktree not a clone?

## Exercise 2 — Observe branch safety

**Scope**: Learn the guardrail rather than bypassing it.

```bash
git worktree add ../wt-same-branch main
```

If `main` is already checked out in the current worktree, Git should refuse. Do not bypass this for normal agent work.

**Verify**:
- You can explain the refusal.
- You can choose `-b new-branch` or `--detach` instead.

**Teach-back**: Why is this guardrail useful for AI agents?

## Exercise 3 — Agent split simulation

**Scope**: Simulate three agents without actually delegating.

```bash
git worktree add ../repo-agent-docs -b agent/docs-playbook
git worktree add ../repo-agent-a -b agent/slice-a
git worktree add ../repo-agent-b -b agent/slice-b
```

In each worktree, make a tiny non-overlapping change such as a scratch note. Then from the main worktree, inspect each branch.

**Verify**:
- Each path has a unique branch.
- Each branch has a unique intended scope.
- You can compare diffs without entering the wrong path.

**Agent prompt template**:

```text
You are working in absolute path: <path>.
Branch: <branch>.
Task scope: <scope>.
Do not edit: <forbidden paths>.
Before final answer: run git status, relevant tests, and report changed files with absolute paths.
Do not push or create PRs without explicit consent.
```

## Exercise 4 — Cleanup and recovery drill

**Scope**: Practice normal cleanup; understand abnormal cleanup.

```bash
git worktree remove ../wt-demo-one
git worktree remove ../wt-scratch
git worktree list
```

Read about, but do not force unless in a disposable repo:

```bash
git worktree prune --dry-run
git worktree repair <path>
```

**Verify**:
- Clean worktrees remove without `--force`.
- You can explain when `prune` is safe.
- You can explain why `repair` exists after manual moves.

## Stretch goals

- Enable `extensions.worktreeConfig` in a disposable repo and set one worktree-specific config value.
- Try `git worktree move` and then inspect `git worktree list --porcelain`.
- Use `git range-diff` to compare two agent branches that evolved from the same base.
