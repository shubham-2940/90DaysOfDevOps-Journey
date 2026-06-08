# Day 12 – Revision & Consolidation (Days 01–11)

## Objective

Today was a revision day to reinforce the Linux and DevOps fundamentals learned during Days 01–11 of the #90DaysOfDevOps challenge.

Instead of learning new concepts, I reviewed previous topics, reran important commands, and verified my understanding through hands-on practice.

---

# Review of Days 01–11

## 1. Mindset & Learning Plan Review

I revisited my Day 01 learning goals and confirmed that my focus remains on:

* Linux Fundamentals
* Cloud Computing Basics
* Networking
* Docker
* Kubernetes
* CI/CD
* Infrastructure as Code
* AWS & DevOps Tools

### Progress So Far

✅ Comfortable with basic Linux navigation

✅ Can manage files and directories

✅ Understand Linux permissions and ownership

✅ Can create users and groups

✅ Can inspect processes and services

✅ Able to troubleshoot common Linux issues

---

# 2. Processes & Services Review

## Commands Practiced

### Check Running Processes

```bash
ps aux
```

### Check Service Status

```bash
systemctl status ssh
```

### View Service Logs

```bash
journalctl -u ssh
```

## Observations

* `ps aux` displays currently running processes.
* `systemctl status` helps verify whether a service is active.
* `journalctl` provides logs useful for troubleshooting service issues.

---

# 3. File Operations Revision

## Create Directories

```bash
mkdir revision-practice
```

## Create Files

```bash
touch notes.txt
```

## Append Text

```bash
echo "Linux Revision Day" >> notes.txt
```

## Copy Files

```bash
cp notes.txt backup.txt
```

## View File Details

```bash
ls -l
```

## Change Permissions

```bash
chmod 755 notes.txt
```

### Observation

Understanding permissions and file operations has become much easier after repeated practice.

---

# 4. Ownership & Group Management Review

## Create User

```bash
sudo useradd revisionuser
```

## Create Group

```bash
sudo groupadd devopsgrp
```

## Add User to Group

```bash
sudo usermod -aG devopsgrp revisionuser
```

## Verify User Information

```bash
id revisionuser
```

## Change Ownership

```bash
sudo chown revisionuser:devopsgrp notes.txt
```

## Verify Ownership

```bash
ls -l notes.txt
```

### Observation

Ownership controls who can access files while permissions control what actions they can perform.

---

# Cheat Sheet Refresh

## Five Commands I Would Use First During an Incident

### Check Current Directory

```bash
pwd
```

### List Files

```bash
ls -la
```

### Check Running Processes

```bash
ps aux
```

### Check Service Status

```bash
systemctl status <service-name>
```

### View Logs

```bash
journalctl -xe
```

These commands provide quick visibility into system state and help identify issues faster.

---

# Mini Self-Check

## 1. Which 3 commands save you the most time right now and why?

### ls -la

Shows files, directories, permissions, and ownership in one command.

### ps aux

Quickly identifies running processes and helps with troubleshooting.

### systemctl status

Provides immediate information about the health of services.

---

## 2. How do you check if a service is healthy?

The first commands I would run are:

```bash
systemctl status <service-name>
```

```bash
journalctl -u <service-name>
```

```bash
ps aux | grep <service-name>
```

These commands help verify service status, logs, and running processes.

---

## 3. How do you safely change ownership and permissions without breaking access?

First verify current ownership:

```bash
ls -l file.txt
```

Then update ownership:

```bash
sudo chown user:group file.txt
```

Finally update permissions:

```bash
chmod 644 file.txt
```

Always verify changes afterward using:

```bash
ls -l file.txt
```

---

## 4. What will you focus on improving in the next 3 days?

* Linux troubleshooting skills
* Networking fundamentals
* Docker basics
* Faster command-line navigation
* Reading logs and identifying issues efficiently

---

# Key Takeaways

* Linux permissions and ownership are critical for security.
* Service troubleshooting starts with status checks and logs.
* User and group management is essential for system administration.
* Consistent hands-on practice improves command retention.
* Documentation helps reinforce learning and track progress.

---

# Why This Matters for DevOps

The first eleven days established the foundation required for DevOps engineering:

* Linux Administration
* User Management
* File Permissions
* Service Monitoring
* Troubleshooting
* System Navigation

These skills are used daily by DevOps engineers while managing servers, applications, and cloud infrastructure.

---

## Revision Day Status

✅ Reviewed Linux fundamentals

✅ Revisited permissions and ownership

✅ Practiced user and group management

✅ Reviewed process and service monitoring

✅ Completed self-assessment

🚀 Ready for Day 13
