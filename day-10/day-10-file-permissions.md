# Day 10 – Linux File Permissions and Access Control

## Objective

Today I practiced Linux file creation, file reading, permission management, and access control.

The goal was to understand how Linux permissions work and how they affect file execution, modification, and directory access.

---

# Task 1 – Create Files

## Create Working Directory

```bash
mkdir file-permission-operations
cd file-permission-operations
```

### Output

```bash
~/file-permission-operations
```

---

## Create Empty File

```bash
touch devops.txt
```

### Verify

```bash
ls -l
```

### Output

```bash
-rw-rw-r-- 1 ubuntu ubuntu 0 Jun 6 16:16 devops.txt
```

---

## Create notes.txt with Content

```bash
echo "This file is created using echo command" >> notes.txt
```

### Verify

```bash
cat notes.txt
```

### Output

```text
This file is created using echo command
```

---

## Create script.sh

```bash
vim script.sh
```

### Content

```bash
echo "Hello DevOps Learners"
```

### Verify

```bash
ls -l
```

### Output

```bash
-rw-rw-r-- 1 ubuntu ubuntu 42 Jun 6 16:18 script.sh
```

---

# Task 2 – Read Files

## Read notes.txt

```bash
cat notes.txt
```

### Output

```text
This file is created using echo command
```

---

## View First 5 Lines of /etc/passwd

```bash
head -n 5 /etc/passwd
```

### Output

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
```

---

## View Last 5 Lines of /etc/passwd

```bash
tail -n 5 /etc/passwd
```

### Output

```text
hamza:x:1001:1001::/home/hamza:/bin/sh
tokyo:x:1002:1003::/home/tokyo:/bin/sh
berlin:x:1003:1004::/home/berlin:/bin/sh
professor:x:1004:1005::/home/professor:/bin/sh
nairobi:x:1005:1008::/home/nairobi:/bin/sh
```

---

# Task 3 – Understand Permissions

## Check Permissions

```bash
ls -l devops.txt notes.txt script.sh
```

### Output

```bash
-rw-rw-r-- 1 ubuntu ubuntu 0  devops.txt
-rw-rw-r-- 1 ubuntu ubuntu 40 notes.txt
-rw-rw-r-- 1 ubuntu ubuntu 42 script.sh
```

## Permission Breakdown

```text
-rw-rw-r--
```

| Section | Meaning                  |
| ------- | ------------------------ |
| rw-     | Owner can read and write |
| rw-     | Group can read and write |
| r--     | Others can only read     |

### Numeric Values

```text
r = 4
w = 2
x = 1
```

Examples:

```text
755 = rwxr-xr-x
644 = rw-r--r--
640 = rw-r-----
444 = r--r--r--
```

---

# Task 4 – Modify Permissions

## Attempt to Execute script.sh

```bash
./script.sh
```

### Error

```bash
-bash: ./script.sh: Permission denied
```

### Reason

The script did not have execute permission.

Current Permission:

```bash
-rw-rw-r--
```

No execute bit (`x`) was present.

---

## Fix the Issue

```bash
chmod 764 script.sh
```

### Verify

```bash
ls -l script.sh
```

### Output

```bash
-rwxrw-r-- 1 ubuntu ubuntu 42 script.sh
```

---

## Execute Again

```bash
./script.sh
```

### Output

```text
Hello DevOps Learners
```

### Troubleshooting Summary

Problem:

```text
Permission denied
```

Solution:

```bash
chmod 764 script.sh
```

Added execute permission for the owner.

---

## Make devops.txt Read Only

```bash
chmod 444 devops.txt
```

### Verify

```bash
ls -l devops.txt
```

### Output

```bash
-r--r--r-- 1 ubuntu ubuntu 0 devops.txt
```

---

## Set notes.txt Permission to 640

```bash
chmod 640 notes.txt
```

### Verify

```bash
ls -l notes.txt
```

### Output

```bash
-rw-r----- 1 ubuntu ubuntu 40 notes.txt
```

---

## Create Directory with 755 Permission

```bash
mkdir -m 755 project
```

### Verify

```bash
ls -l
```

### Output

```bash
drwxr-xr-x 2 ubuntu ubuntu 4096 project
```

---

# Task 5 – Test Permissions

## Execute Non-Executable File

```bash
./devops.txt
```

### Error

```bash
-bash: ./devops.txt: Permission denied
```

### Reason

The file does not have execute permission.

Permission:

```bash
-r--r--r--
```

---

## Edit Read-Only File

Open file:

```bash
vim devops.txt
```

Attempt to modify and save.

### Warning Received

```text
W10: Warning: Changing a readonly file
```

### Reason

The file was set to read-only using:

```bash
chmod 444 devops.txt
```

Linux prevented modification because write permission was removed.

---

# Challenges Faced

## Challenge 1

Could not execute script.sh.

### Error

```bash
Permission denied
```

### Solution

Added execute permission using:

```bash
chmod 764 script.sh
```

---

## Challenge 2

Could not execute devops.txt.

### Error

```bash
Permission denied
```

### Solution

Understood that normal text files are not executable unless execute permission is granted and the file contains executable commands.

---

## Challenge 3

Vim displayed readonly warning while editing devops.txt.

### Error

```text
W10: Warning: Changing a readonly file
```

### Solution

Learned that write permissions were removed with:

```bash
chmod 444 devops.txt
```

Readonly files cannot be modified without changing permissions.

---

# What I Learned

* Linux permissions are divided into Owner, Group, and Others.
* Permission values are represented using rwx notation and numeric values.
* Execute permission is required to run scripts.
* Read-only files prevent accidental modifications.
* The chmod command is used to manage file and directory permissions.
* Directories use execute permission to allow access to their contents.

---

# Why This Matters for DevOps

File permissions are a fundamental part of Linux system administration.

As a DevOps Engineer, managing permissions correctly helps:

* Secure applications and servers
* Protect sensitive files
* Control user access
* Run automation scripts safely
* Prevent unauthorized changes

Understanding Linux permissions is essential for working with servers, cloud environments, containers, and automation tools.
