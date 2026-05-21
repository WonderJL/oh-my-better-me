# Resources: Git Worktree for Agent Multitasking

## Foundational reading

- Official `git worktree` documentation — https://git-scm.com/docs/git-worktree
  - Read: Description, Commands, Details, Refs, Configuration File.
- Official Git repository layout — https://git-scm.com/docs/gitrepository-layout
  - Read only enough to understand `$GIT_DIR`, `$GIT_COMMON_DIR`, and linked-worktree metadata.
- Official `git config` documentation — https://git-scm.com/docs/git-config
  - Search within the page for `worktreeConfig`, `worktree.guessRemote`, and `worktree.useRelativePaths`.

## Workflow context

- GitHub blog: Git 2.5 worktree announcement — https://github.blog/open-source/git/git-2-5-including-multiple-worktrees-and-triangular-workflows/
  - Useful for motivation: parallel branches/tests without multiple clones.

## Command references

- `git branch` — https://git-scm.com/docs/git-branch
- `git checkout` — https://git-scm.com/docs/git-checkout
- `git diff` — https://git-scm.com/docs/git-diff
- `git range-diff` — https://git-scm.com/docs/git-range-diff
- `git merge` — https://git-scm.com/docs/git-merge

## Source to inspect later

- Git's own test suite for worktree behavior: https://github.com/git/git/tree/master/t
  - Look for files named around `worktree`, `branch`, and `checkout` if you want implementation-level confidence.

## What not to over-read today

- Do not spend the day on Git internals beyond the linked-worktree layout.
- Do not read every option in `git config`; focus only on worktree-related settings.
- Do not optimize folder conventions before you can create/list/remove worktrees fluently.
