---
name: merge-to-main
description: Squash-merge the current feature branch into main, push, and delete both the local and remote branch. Use when the user says "merge to main", "merge this branch", "finish this branch", or asks to land a completed feature branch cleanly. Produces a single polished commit on main from all branch commits.
---

## Workflow

### 1. Preflight checks

```bash
git status                          # must be clean — stop if uncommitted changes exist
git branch --show-current           # save as BRANCH; stop if already on main
git log main..HEAD --oneline        # preview commits to be squashed
```

If uncommitted changes exist, **stop and tell the user**. Do not stash silently.

### 2. Update both branches

```bash
git fetch origin
git rebase origin/main              # rebase BRANCH onto latest main
```

If rebase conflicts occur, stop and report them. Do not auto-resolve.

### 3. Squash merge

```bash
git checkout main
git pull origin main                # ensure local main is current
git merge --squash BRANCH
```

### 4. Craft the commit message

Read the squashed commits and diff to write a meaningful message:

```bash
git log BRANCH --not main --oneline   # all commits being squashed
git diff --cached --stat              # staged changes summary
```

- Subject: concise summary ≤72 chars
- Body: bullet points for significant sub-changes (optional)
- No "Co-Authored-By" or AI attribution

```bash
git commit -m "subject line"
```

### 5. Push main

```bash
git push origin main
```

### 6. Delete the branch

```bash
git branch -D BRANCH                # -D required after squash (no real merge commit recorded)
git push origin --delete BRANCH     # skip silently if remote branch doesn't exist
```

### 7. Confirm

Report: `✓ Merged BRANCH into main and deleted branch (local + remote).`
