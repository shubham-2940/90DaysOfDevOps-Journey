# Day 25 – Git Reset vs Revert & Branching Strategies

## Objective
Understand safe ways to undo changes and learn common Git branching strategies.

---

# Task 1 – Git Reset

I created three commits:

```
Commit A
Commit B
Commit C
```

## git reset --soft

```bash
git reset --soft HEAD~1
```

Observation:
- Commit removed
- Changes remained staged

## git reset --mixed

```bash
git reset --mixed HEAD~1
```

Observation:
- Commit removed
- Changes remained in working directory
- Changes became unstaged

## git reset --hard

```bash
git reset --hard HEAD~1
```

Observation:
- Commit removed
- Staging area cleared
- Working directory restored
- Changes permanently discarded

### Comparison

| Option | Commit | Staged | Working Directory |
|--------|--------|--------|-------------------|
| --soft | Removed | Yes | Preserved |
| --mixed | Removed | No | Preserved |
| --hard | Removed | No | Deleted |

### Which is destructive?
`git reset --hard` because it permanently deletes uncommitted work.

### When to use?
- soft → modify last commit
- mixed → unstage changes
- hard → discard local work

### Should reset be used on pushed commits?
No. It rewrites history and can disrupt collaborators.

---

# Task 2 – Git Revert

I created:

```
Commit X
Commit Y
Commit Z
```

I reverted the middle commit.

```bash
git revert <hash-of-Y>
```

While reverting, I experienced a merge conflict because Commit Z modified the same file after Commit Y. After resolving the conflict, Git created a new commit:

```
Revert "Commit Y"
```

### Observation
Commit Y remained in history, but its changes were reversed.

### Reset vs Revert

| Git Reset | Git Revert |
|------------|------------|
| Rewrites history | Preserves history |
| Removes commits | Creates undo commit |
| Unsafe for shared branches | Safe for shared branches |

### Why is revert safer?
It never changes existing commit history, making collaboration easier.

### When to use?
- Reset → local cleanup
- Revert → shared repositories

---

# Task 3 – Reset vs Revert Summary

| Feature | git reset | git revert |
|---------|-----------|------------|
| Removes commit from history | Yes | No |
| Creates new commit | No | Yes |
| Rewrites history | Yes | No |
| Safe for pushed branches | No | Yes |
| Best use | Local changes | Shared repositories |

---

# Task 4 – Branching Strategies

## 1. GitFlow

### Flow

```text
main
 |
develop
 |---feature/*
 |---release/*
 |---hotfix/*
```

### Usage
Large teams with scheduled releases.

### Pros
- Structured
- Stable releases
- Supports parallel work

### Cons
- More complex
- Many long-lived branches

---

## 2. GitHub Flow

### Flow

```text
main
 |
feature
 |
Pull Request
 |
main
```

### Usage
Continuous deployment and web applications.

### Pros
- Simple
- Fast
- Easy collaboration

### Cons
- Less suitable for multiple release versions

---

## 3. Trunk-Based Development

### Flow

```text
main
 |\
 | feature
 |/
main
```

Short-lived branches merged frequently.

### Usage
High-frequency deployment teams.

### Pros
- Fewer merge conflicts
- Fast integration

### Cons
- Requires strong testing and CI/CD

---

# Answers

## Best for startup?
**GitHub Flow**, because it is simple and supports rapid delivery.

## Best for large teams?
**GitFlow**, because it supports planned releases and multiple parallel development streams.

## Open-source example
The React project primarily follows a GitHub Flow–style workflow with feature branches, pull requests, and frequent merges into the main branch.

---

# Key Learnings

- Reset rewrites history.
- Revert preserves history.
- Hard reset permanently removes local changes.
- Revert is preferred for shared repositories.
- Different projects benefit from different branching strategies.
