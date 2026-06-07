# Day 11 – Linux File Ownership & Group Management

## Objective

Today I practiced Linux file ownership and group management concepts. The goal was to understand how Linux controls access to files and directories through users and groups, and how ownership can be modified using commands such as `chown` and `chgrp`.

---

# Task 1 – Understanding File Ownership

## Commands Used

```bash
pwd
ls -l
```

## Output Observed

Example:

```bash
-rwxrw-r-- 1 ubuntu ubuntu 35 Jun 4 11:03 demo.txt
```

### Understanding the Columns

```text
-rwxrw-r-- 1 ubuntu ubuntu 35 Jun 4 11:03 demo.txt
             |      |
           Owner   Group
```

### Difference Between Owner and Group

* **Owner** is the user who owns the file.
* **Group** is a collection of users who can share access to the file.
* Linux permissions can be assigned separately to:

  * Owner
  * Group
  * Others

---

# Task 2 – Basic chown Operations

## Create File

```bash
touch devops-file.txt
ls -l devops-file.txt
```

### Initial Ownership

```bash
-rw-rw-r-- 1 ubuntu ubuntu 0 Jun 6 16:34 devops-file.txt
```

## Change Owner to tokyo

```bash
sudo chown tokyo devops-file.txt
ls -l devops-file.txt
```

### Result

```bash
-rw-rw-r-- 1 tokyo ubuntu 0 Jun 6 16:34 devops-file.txt
```

## Change Owner to berlin

```bash
sudo chown berlin devops-file.txt
ls -l devops-file.txt
```

### Result

```bash
-rw-rw-r-- 1 berlin ubuntu 0 Jun 6 16:34 devops-file.txt
```

### Learning

`chown` changes the ownership of a file or directory.

---

# Task 3 – Basic chgrp Operations

## Create File

```bash
touch team-notes.txt
```

## Create Group

```bash
sudo groupadd heist-team
```

## Change Group Ownership

```bash
sudo chgrp heist-team team-notes.txt
```

## Verify

```bash
ls -l team-notes.txt
```

### Result

```bash
-rw-rw-r-- 1 ubuntu heist-team 0 Jun 6 16:37 team-notes.txt
```

### Learning

`chgrp` changes only the group ownership of a file.

---

# Task 4 – Combined Owner and Group Change

## Create File

```bash
touch project-config.yaml
```

## Change Owner and Group Together

```bash
sudo chown professor:heist-team project-config.yaml
```

## Create Directory

```bash
mkdir app-logs
```

## Change Directory Ownership

```bash
sudo chown berlin:heist-team app-logs
```

## Verify

```bash
ls -l
```

### Result

```bash
drwxrwxr-x 2 berlin    heist-team app-logs
-rw-rw-r-- 1 professor heist-team project-config.yaml
-rw-rw-r-- 1 ubuntu    heist-team team-notes.txt
```

### Learning

Using the syntax below changes owner and group in a single command:

```bash
sudo chown owner:group filename
```

---

# Task 5 – Recursive Ownership

## Create Directory Structure

```bash
mkdir -p heist-project/vault
mkdir -p heist-project/plans
```

## Create Files

```bash
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf
```

## Create Group

```bash
sudo groupadd planners
```

## Apply Recursive Ownership

```bash
sudo chown -R professor:planners heist-project/
```

## Verify

```bash
ls -lR heist-project/
```

### Result

```bash
heist-project/
├── plans
└── vault

plans/
└── strategy.conf

vault/
└── gold.txt
```

All directories and files were owned by:

```text
Owner : professor
Group : planners
```

### Learning

The `-R` flag applies ownership changes recursively to all files and subdirectories.

---

# Task 6 – Practice Challenge

## Create Groups

```bash
sudo groupadd vault-team
sudo groupadd tech-team
```

## Create Directory

```bash
mkdir bank-heist
```

## Create Files

```bash
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt
```

## Assign Ownership

### Access Codes

```bash
sudo chown tokyo:vault-team bank-heist/access-codes.txt
```

### Blueprints

```bash
sudo chown berlin:tech-team bank-heist/blueprints.pdf
```

### Escape Plan

```bash
sudo chown nairobi:vault-team bank-heist/escape-plan.txt
```

## Verify

```bash
ls -l bank-heist/
```

### Final Output

```bash
-rw-rw-r-- 1 tokyo   vault-team  access-codes.txt
-rw-rw-r-- 1 berlin  tech-team   blueprints.pdf
-rw-rw-r-- 1 nairobi vault-team  escape-plan.txt
```

---

# Challenges Faced and Troubleshooting

## Issue 1 – Incorrect Directory Name

### Command

```bash
touch heist-project/valut/gold.txt
```

### Error

```bash
No such file or directory
```

### Cause

Directory name was typed incorrectly.

```text
valut ❌
vault ✅
```

### Fix

```bash
touch heist-project/vault/gold.txt
```

---

## Issue 2 – Wrong Command

### Command

```bash
sudo grouadd planners
```

### Error

```bash
command not found
```

### Cause

Typographical mistake.

### Fix

```bash
sudo groupadd planners
```

---

## Issue 3 – Invalid Group Error

### Command

```bash
sudo chown -R professor:planners heist-project/
```

### Error

```bash
invalid group
```

### Cause

Group did not exist yet.

### Fix

```bash
sudo groupadd planners
sudo chown -R professor:planners heist-project/
```

---

## Issue 4 – Wrong Group Name

### Error

```bash
invalid group
```

### Cause

Group name typo while assigning ownership.

### Fix

Verified group names and re-ran the command with the correct group.

---

## Issue 5 – Incorrect File Path

### Error

```bash
No such file or directory
```

### Cause

File path was incomplete.

### Fix

Used the full path:

```bash
bank-heist/access-codes.txt
```

---

# Commands Used

```bash
ls -l
ls -lR
pwd

touch

groupadd

chgrp

chown

chown owner:group filename

chown -R owner:group directory
```

---

# What I Learned

* Every Linux file has an owner and a group.
* `chown` changes ownership.
* `chgrp` changes group ownership.
* Owner and group can be changed together using a single command.
* Recursive ownership changes are useful for project directories.
* Reading error messages carefully helps identify mistakes quickly.
* Most Linux issues are caused by typos, incorrect paths, or missing users/groups.

---

# Why This Matters for DevOps

Understanding ownership and permissions is essential for:

* Server administration
* Application deployments
* Shared team access
* Log management
* Security hardening
* CI/CD pipeline execution
* Managing production Linux systems

Proper ownership ensures that applications and users have the right level of access while maintaining security and stability.
