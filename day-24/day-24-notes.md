# Day 24 – Advanced Git: Merge, Rebase, Stash & Cherry Pick

## Objective
Learn advanced Git workflows including merging, rebasing, stash management and cherry-picking.

---

# Task 1 – Git Merge

## Fast-Forward Merge
### Observation
I created a `feature-login` branch from `master`, made two commits, and merged it back into `master`. Since no new commits were added to `master`, Git performed a **Fast-Forward Merge** by simply moving the branch pointer forward.

### What is a Fast-Forward Merge?
A fast-forward merge happens when the target branch has not moved ahead since the feature branch was created. Git only moves the branch pointer without creating a merge commit.

### Merge Commit
Next, I created `feature-signup`, added commits, then created another commit on `master` before merging.

### Observation
Git created a **merge commit** because both branches had diverged.

### When does Git create a merge commit?
Git creates a merge commit when both branches contain unique commits and their histories need to be combined.

### Merge Conflict
I intentionally modified the same line in both branches. Git was unable to merge automatically and asked me to resolve the conflict manually.

### What is a Merge Conflict?
A merge conflict occurs when Git cannot automatically determine which change should be kept.

---

# Task 2 – Git Rebase

## Observation
I created `feature-dashboard`, added multiple commits, then added another commit to `master`. After running:

```bash
git rebase master
```

Git replayed my feature branch commits on top of the latest `master`.

### What does rebase do?
Rebase moves the base of a branch to another commit and replays its commits one by one, creating new commit hashes.

### Merge vs Rebase

Merge

```text
A---B---C------M
     \        /
      D---E---
```

Rebase

```text
A---B---C---D'---E'
```

### Why not rebase shared commits?
Rebase rewrites commit history. If commits are already pushed and shared, teammates may experience conflicts because commit hashes change.

### When to use Rebase vs Merge?
- Rebase: Clean local history before merging.
- Merge: Preserve branch history in collaborative work.

---

# Task 3 – Squash Merge vs Regular Merge

## Observation
I created `feature-profile` with several small commits and merged it using:

```bash
git merge --squash feature-profile
```

Only **one commit** was added to `master`.

Then I created `feature-settings` and merged it normally.

### What does squash merging do?
It combines all commits from a feature branch into a single commit before adding them to the target branch.

### Squash Merge vs Regular Merge

| Squash Merge | Regular Merge |
|--------------|---------------|
| One commit added | All commits preserved |
| Cleaner history | Detailed history |
| Easier to read | Better for debugging |

### Trade-off
Squashing removes individual commit history, making it harder to trace intermediate development.

---

# Task 4 – Git Stash

## Observation
I modified a tracked file without committing it. I used:

```bash
git stash
```

Then switched branches, completed another task, returned, and restored my work using:

```bash
git stash pop
```

I also created multiple stashes and viewed them using:

```bash
git stash list
```

Applied a specific stash:

```bash
git stash apply stash@{1}
```

### git stash pop vs git stash apply

| git stash pop | git stash apply |
|----------------|-----------------|
| Applies and removes stash | Applies but keeps stash |

### Real-world use cases
- Emergency production bug
- Branch switching
- Pull latest changes
- Temporary work-in-progress

---

# Task 5 – Cherry Pick

## Observation
I created `feature-hotfix` with three commits.

```
Fix login bug
Fix security bug
Optimize performance
```

I switched to `master` and cherry-picked only the second commit.

```bash
git cherry-pick <commit-hash>
```

Only the selected commit appeared on `master` with a new commit hash.

### What does cherry-pick do?
Copies a specific commit from one branch to another without merging the whole branch.

### Real-world uses
- Backport hotfixes
- Apply urgent bug fixes
- Move accidental commits

### Possible issues
- Merge conflicts
- Duplicate commits
- Dependency on previous commits

---

# Key Learnings

- Fast-forward merge moves the branch pointer.
- Merge commit preserves branch history.
- Rebase creates a linear history.
- Squash merge keeps history clean.
- Stash temporarily saves work.
- Cherry-pick copies selected commits.

