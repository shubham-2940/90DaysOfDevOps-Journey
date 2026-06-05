# Day 09 – Linux User & Group Management Challenge

## Objective

Today's goal was to gain hands-on experience with Linux user and group management by completing practical administration tasks.

The focus areas included:

* Creating users and setting passwords
* Creating and managing groups
* Assigning users to groups
* Configuring shared directories
* Managing file permissions
* Testing collaborative access using Linux groups

---

# Task 1: Create Users

## Create Users with Home Directories

```bash
sudo useradd -m tokyo
sudo useradd -m berlin
sudo useradd -m professor
```

## Set Passwords

```bash
sudo passwd tokyo
sudo passwd berlin
sudo passwd professor
```

## Verification

Check user entries:

```bash
grep -E "tokyo|berlin|professor" /etc/passwd
```

Check home directories:

```bash
ls -l /home
```

### Result

* Successfully created users:

  * tokyo
  * berlin
  * professor
* Home directories were automatically created.

---

# Task 2: Create Groups

## Create Groups

```bash
sudo groupadd developers
sudo groupadd admins
```

## Verification

```bash
grep -E "developers|admins" /etc/group
```

### Result

Successfully created:

* developers
* admins

---

# Task 3: Assign Users to Groups

## Add Users to Groups

Assign tokyo to developers:

```bash
sudo usermod -aG developers tokyo
```

Assign berlin to developers and admins:

```bash
sudo usermod -aG developers berlin
sudo usermod -aG admins berlin
```

Assign professor to admins:

```bash
sudo usermod -aG admins professor
```

## Verification

```bash
groups tokyo
groups berlin
groups professor
```

### Expected Output

```text
tokyo : tokyo developers

berlin : berlin developers admins

professor : professor admins
```

### Result

Users were successfully assigned to the required groups.

---

# Task 4: Shared Directory

## Create Shared Directory

```bash
sudo mkdir -p /opt/dev-project
```

## Set Group Ownership

```bash
sudo chgrp developers /opt/dev-project
```

## Set Permissions

```bash
sudo chmod 775 /opt/dev-project
```

## Verify Directory Permissions

```bash
ls -ld /opt/dev-project
```

Expected:

```text
drwxrwxr-x
```

## Test File Creation as tokyo

```bash
su - tokyo
touch /opt/dev-project/tokyo-file.txt
exit
```

## Test File Creation as berlin

```bash
su - berlin
touch /opt/dev-project/berlin-file.txt
exit
```

## Verification

```bash
ls -l /opt/dev-project
```

### Result

Both users successfully created files in the shared directory, confirming proper group permissions.

---

# Task 5: Team Workspace

## Create User

```bash
sudo useradd -m nairobi
sudo passwd nairobi
```

## Create Group

```bash
sudo groupadd project-team
```

## Add Members to Group

```bash
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo
```

## Verify Membership

```bash
groups nairobi
groups tokyo
```

## Create Workspace Directory

```bash
sudo mkdir -p /opt/team-workspace
```

## Assign Group Ownership

```bash
sudo chgrp project-team /opt/team-workspace
```

## Set Permissions

```bash
sudo chmod 775 /opt/team-workspace
```

## Verify Directory

```bash
ls -ld /opt/team-workspace
```

Expected:

```text
drwxrwxr-x
```

## Test as nairobi

```bash
su - nairobi
touch /opt/team-workspace/nairobi-test.txt
exit
```

## Verification

```bash
ls -l /opt/team-workspace
```

### Optional Improvement

Enable SetGID for automatic group inheritance:

```bash
sudo chmod g+s /opt/team-workspace
```

Verify:

```bash
ls -ld /opt/team-workspace
```

Output:

```text
drwxrwsr-x
```

### Result

Successfully configured a shared team workspace with group-based access control.

---

# Commands Used

```bash
useradd
passwd
groupadd
usermod
groups
grep
mkdir
chgrp
chmod
touch
su
ls
```

---

# Challenges Faced

### 1. Understanding Primary vs Secondary Groups

Initially, it was confusing why users did not immediately show membership in newly assigned groups.

**Solution:**
Used the `groups` command and learned that group changes may require a new login session.

### 2. Shared Directory Permissions

Understanding how ownership and permissions work together required practice.

**Solution:**
Verified ownership using `ls -ld` and tested access by switching users.

### 3. Group-Based Collaboration

Learned that users need both:

* Membership in the group
* Proper directory permissions

for successful collaboration.

---

# What I Learned

* How to create Linux users and home directories
* How to create and manage groups
* How to assign users to multiple groups
* How Linux permissions work with users and groups
* How to configure shared directories for team collaboration
* How to verify access using real user accounts
* Importance of group ownership in multi-user environments

---

# Why This Matters for DevOps

User and group management is a fundamental Linux administration skill used daily by DevOps Engineers.

This exercise taught me how to:

* Manage access securely
* Organize users into teams
* Configure shared project workspaces
* Control permissions using Linux groups
* Implement collaboration practices commonly used on production servers

These concepts are essential for managing Linux servers, cloud instances, CI/CD environments, and enterprise infrastructure.

---

# Summary

Completed all five Linux User & Group Management challenges successfully:

✅ Created users and passwords

✅ Created groups

✅ Assigned users to multiple groups

✅ Configured shared project directories

✅ Tested collaborative file access

✅ Practiced real-world Linux administration tasks

Day 09 strengthened my understanding of Linux user management and prepared me for more advanced system administration and DevOps concepts.
