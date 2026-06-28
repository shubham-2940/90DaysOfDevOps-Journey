# Day 23 - Git Branching & Working with GitHub

## What is a branch in Git?

A branch is an independent line of development in Git. It allows me to work on new features, bug fixes, or experiments without affecting the main branch of the project.

Every branch has its own commit history. Once the work is completed and tested, it can be merged back into the main branch.

### Example

```
master
   │
   ├── feature-1
   │      ├── Commit A
   │      └── Commit B
   │
   └── feature-2
          └── Commit C
```

This allows multiple developers to work on different features simultaneously.

---

# Why do we use branches instead of committing everything to the main branch?

If everyone commits directly to the main branch:

* Bugs may break the project.
* New features may be incomplete.
* Team members can overwrite each other's work.
* It becomes difficult to maintain a stable version of the application.

Using branches allows us to:

* Develop features independently.
* Fix bugs without affecting production code.
* Test new ideas safely.
* Merge only tested and approved code into the main branch.

This keeps the main branch stable and production-ready.

---

# What is HEAD in Git?

HEAD is a special pointer that indicates the current branch and the latest commit that I am working on.

Whenever I switch branches, HEAD automatically points to the latest commit of that branch.

For example:

```
master
   │
   ▼
HEAD
```

After switching to feature-1:

```
feature-1
     │
     ▼
    HEAD
```

I can check my current branch using:

```bash
git branch
```

The branch marked with `*` is where HEAD is currently pointing.

---

# What happens to your files when you switch branches?

When I switch branches, Git updates my working directory to match the latest commit of the selected branch.

For example:

* If `feature-1` contains a file that does not exist on `master`, that file disappears when I switch back to `master`.
* If `feature-1` contains new changes, those changes are visible only on that branch until they are merged.

Git automatically updates the files based on the selected branch.

---

# Difference between git switch and git checkout

## git checkout

`git checkout` is the older command.

It is used for:

* Switching branches
* Restoring files
* Checking out commits

Since it performs multiple tasks, it can sometimes be confusing.

Example:

```bash
git checkout feature-1
```

Create and switch:

```bash
git checkout -b feature-2
```

---

## git switch

`git switch` was introduced to make branch switching simpler.

It is used only for switching branches.

Examples:

```bash
git switch feature-1
```

Create and switch:

```bash
git switch -c feature-2
```

### Summary

| git checkout       | git switch             |
| ------------------ | ---------------------- |
| Older command      | Newer command          |
| Multiple purposes  | Only switches branches |
| Can restore files  | Cannot restore files   |
| Slightly confusing | Easier for beginners   |

---

# What is the difference between origin and upstream?

## Origin

`origin` is the default name of my remote repository.

It usually points to my own GitHub repository where I can push my commits.

Example:

```
origin
↓
https://github.com/myusername/devops-git-practice.git
```

I use it for:

* git push
* git pull
* git fetch

---

## Upstream

`upstream` refers to the original repository from which I created a fork.

It is mainly used in open-source projects.

Example:

```
Original Repository
        │
        ▼
    upstream

My Fork
        │
        ▼
      origin
```

I use upstream to fetch the latest changes from the original project.

Example:

```bash
git fetch upstream
```

---

### Difference

| Origin               | Upstream                  |
| -------------------- | ------------------------- |
| My GitHub repository | Original repository       |
| I push my code here  | I fetch updates from here |
| I have write access  | Usually read-only         |

---

# What is the difference between git fetch and git pull?

## git fetch

`git fetch` downloads the latest changes from the remote repository without merging them into my current branch.

It updates only the remote-tracking branches.

Example:

```bash
git fetch origin
```

---

## git pull

`git pull` downloads the latest changes and immediately merges them into the current branch.

It is equivalent to:

```bash
git fetch
git merge
```

Example:

```bash
git pull origin master
```

---

### Difference

| git fetch                  | git pull                         |
| -------------------------- | -------------------------------- |
| Downloads changes only     | Downloads and merges changes     |
| Safe for reviewing updates | Immediately updates local branch |
| No merge performed         | Merge happens automatically      |

---

# What is the difference between Clone and Fork?

## Clone

A clone creates a local copy of a Git repository on my computer.

Example:

```bash
git clone https://github.com/git/git.git
```

It downloads:

* Files
* Commit history
* Branches

---

## Fork

A fork creates a copy of someone else's repository under my GitHub account.

After forking, I clone my own fork.

Example:

```
Original Repository
        │
      Fork
        │
Clone to Local Machine
```

---

### Difference

| Clone                                                     | Fork                                                              |
| --------------------------------------------------------- | ----------------------------------------------------------------- |
| Local copy                                                | GitHub copy                                                       |
| Used for repositories I own or can contribute to directly | Used for contributing to repositories without direct write access |
| Exists only on my computer                                | Exists on GitHub                                                  |

---

# When would you clone vs fork?

## Use Clone when

* Working on my own repository
* Working in a team repository
* I already have write access
* I simply need a local copy

---

## Use Fork when

* Contributing to open-source projects
* I don't have write access
* I want my own copy on GitHub
* I plan to create a Pull Request

---

# After forking, how do you keep your fork in sync with the original repository?

### Step 1

Add the original repository as upstream.

```bash
git remote add upstream https://github.com/original-owner/project.git
```

### Step 2

Fetch latest changes.

```bash
git fetch upstream
```

### Step 3

Switch to master.

```bash
git switch master
```

### Step 4

Merge upstream changes.

```bash
git merge upstream/master
```

### Step 5

Push updated code to my fork.

```bash
git push origin master
```

This keeps my fork updated with the latest changes from the original project.

---

# Commands I Practiced Today

| Command                        | Purpose                               |
| ------------------------------ | ------------------------------------- |
| `git branch`                   | List local branches                   |
| `git branch -a`                | List all branches                     |
| `git branch feature-1`         | Create a branch                       |
| `git checkout feature-1`       | Switch branch (old method)            |
| `git checkout -b feature-2`    | Create and switch branch              |
| `git switch feature-1`         | Switch branch (new method)            |
| `git switch -c feature-2`      | Create and switch branch (new method) |
| `git branch -d feature-2`      | Delete merged branch                  |
| `git branch -D feature-2`      | Force delete branch                   |
| `git remote -v`                | Show configured remotes               |
| `git remote add origin`        | Add GitHub repository                 |
| `git remote add upstream`      | Add original repository               |
| `git push -u origin master`    | Push master branch                    |
| `git push -u origin feature-1` | Push feature branch                   |
| `git pull origin master`       | Pull latest changes                   |
| `git fetch origin`             | Download remote changes               |
| `git clone <url>`              | Clone repository                      |
| `git merge feature-1`          | Merge feature branch into master      |

---

# Practical Activities Completed

* Installed and configured Git.
* Created and switched between multiple branches.
* Used both `git checkout` and `git switch`.
* Created separate commits on different branches.
* Verified that commits remain isolated until merged.
* Deleted unused branches.
* Connected the local repository to GitHub.
* Pushed both `master` and `feature-1` branches.
* Edited files directly on GitHub.
* Pulled changes to the local repository.
* Cloned a public repository.
* Forked the same repository.
* Cloned my fork.
* Configured the `upstream` remote.
* Learned how to synchronize a fork with the original repository.
* Merged `feature-1` into `master` and updated both branches.

---

# Key Learnings

* Branches help isolate development work.
* HEAD always points to the currently checked-out branch.
* `git switch` is the modern and simpler way to switch branches.
* `origin` points to my repository, while `upstream` points to the original repository.
* `git fetch` downloads changes without merging, whereas `git pull` downloads and merges them.
* Cloning creates a local copy, while forking creates a GitHub copy under my account.
* Keeping my fork updated requires fetching changes from the upstream repository.
* Using branches makes collaboration safer and keeps the main branch stable.
* GitHub serves as a remote backup and collaboration platform for Git repositories.

---

# Conclusion

Today I learned one of the most important concepts in Git—branching. I practiced creating, switching, deleting, and merging branches, worked with remote repositories on GitHub, understood the differences between origin and upstream, learned how cloning differs from forking, and explored how to synchronize a fork with the original repository. These concepts form the foundation of collaborative software development and DevOps workflows.
