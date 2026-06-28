# Day 22 - Git Introduction: My Notes

## 1. What is the difference between `git add` and `git commit`?

`git add` is used to move changes from the working directory to the staging area. It tells Git which changes I want to include in my next commit.

`git commit` saves the staged changes permanently in the local Git repository along with a commit message describing what was changed.

### Example

```bash
git add git-commands.md
git commit -m "Added Git setup commands"
```

Here, `git add` stages the file and `git commit` creates a new snapshot of those staged changes.

---

## 2. What does the staging area do? Why doesn't Git commit directly?

The staging area acts like a preparation area before creating a commit. It allows me to review and choose exactly which changes should be included.

Instead of committing every modified file, I can stage only the files I want.

For example:

- I modified three files.
- Only one file is complete.
- I can stage only that file using `git add`.
- Then commit only the finished work.

This helps create clean and meaningful commit history.

---

## 3. What information does `git log` show?

`git log` displays the history of commits in the repository.

It shows:

- Commit ID (SHA hash)
- Author name
- Author email
- Date and time of commit
- Commit message

Example:

```bash
git log
```

For a compact view:

```bash
git log --oneline
```

Example output:

```
ecaea4f Added how to check last 5 commits history/logs.
ad9a014 Updated git-commands.md file.
b38d3ca Added commands to check commit history.
c53fa4f Added how to commit the changes.
f3609e2 Added about how to add any file in staging phase.
cc4226b Created git-commands.md file for practice.
```

This makes it easy to track project history.

---

## 4. What is the `.git/` folder and what happens if you delete it?

The `.git` folder is created when we run:

```bash
git init
```

It contains all the information Git needs to manage the repository, including:

- Commit history
- Branch information
- Repository configuration
- Object database
- References
- Logs

It is the heart of every Git repository.

If the `.git` folder is deleted:

- The project files remain.
- All Git history is lost.
- Branches disappear.
- Commits cannot be recovered from that repository.
- The folder becomes a normal directory instead of a Git repository.

---

## 5. What is the difference between Working Directory, Staging Area, and Repository?

### Working Directory

This is where I create or edit project files.

Example:

- Edit `git-commands.md`
- Create new files
- Delete unwanted files

Changes here are **not tracked** until they are staged.

---

### Staging Area

The staging area stores changes that are ready to be committed.

Files enter the staging area using:

```bash
git add <file>
```

Git uses this area to prepare the next commit.

---

### Repository

The repository contains all committed versions of the project.

Whenever I run:

```bash
git commit -m "message"
```

Git stores a permanent snapshot inside the repository.

The repository also stores:

- Commit history
- Branches
- Tags
- Version history

---

## Git Workflow I Learned

```
Working Directory
        │
        │ Edit files
        ▼
git status
        │
        ▼
git add
        │
        ▼
Staging Area
        │
        ▼
git commit
        │
        ▼
Local Repository
```

---

## Commands I Practiced Today

| Command | Purpose |
|----------|---------|
| `git --version` | Check Git installation |
| `git init` | Initialize a new repository |
| `git config --global user.name` | Configure username |
| `git config --global user.email` | Configure email |
| `git config --list` | Verify Git configuration |
| `git status` | Check repository status |
| `git add` | Stage files |
| `git commit -m` | Create a commit |
| `git log` | View detailed commit history |
| `git log --oneline` | View compact commit history |

---

## Challenges I Faced

- Initially I tried running `git commit` without staging the modified file.
- Git displayed the message:

```
no changes added to commit
```

- I understood that Git only commits staged changes.
- After using `git add .`, the commit was created successfully.
- This helped me understand the purpose of the staging area.

---

## Key Learnings

- Git tracks changes in files over time.
- Every commit creates a snapshot of the project.
- The staging area helps create clean and meaningful commits.
- `git status` is one of the most useful commands because it tells me what Git is expecting next.
- Writing clear commit messages makes the project history easier to understand.
- Keeping multiple small commits is better than one large commit because it makes debugging and tracking changes easier.

---

## Conclusion

Today I learned the complete basic Git workflow:

1. Initialize a repository.
2. Configure Git.
3. Create project files.
4. Check repository status.
5. Stage changes.
6. Commit changes with meaningful messages.
7. View commit history.

I also started building my personal Git command reference (`git-commands.md`), which I will continue updating throughout the upcoming Git learning days.
