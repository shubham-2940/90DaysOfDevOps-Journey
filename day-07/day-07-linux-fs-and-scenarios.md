# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Objective

Today I practiced:

* Linux File System Hierarchy (FHS)
* Understanding important Linux directories
* Finding configuration files and logs
* Service troubleshooting
* CPU troubleshooting
* Log analysis
* File permission troubleshooting

This practice helped me understand where Linux stores system files and how a DevOps Engineer approaches troubleshooting in real-world environments.

---

# Part 1 – Linux File System Hierarchy

Linux follows a hierarchical file system where everything starts from the root directory (`/`).

## Linux File System Structure

| Directory | Purpose                                                         |
| --------- | --------------------------------------------------------------- |
| `/`       | Root directory – starting point of the entire Linux file system |
| `/bin`    | Essential user command binaries                                 |
| `/boot`   | Static files used by the boot loader                            |
| `/dev`    | Device files                                                    |
| `/etc`    | Host-specific system configuration files                        |
| `/home`   | User home directories                                           |
| `/lib`    | Shared libraries required by binaries                           |
| `/media`  | Mount point for removable media                                 |
| `/mnt`    | Temporary mount point for filesystems                           |
| `/opt`    | Optional third-party applications                               |
| `/sbin`   | Essential system binaries                                       |
| `/srv`    | Data served by system services                                  |
| `/tmp`    | Temporary files                                                 |
| `/usr`    | User utilities and applications                                 |
| `/proc`   | Process and kernel information                                  |

---

## Directory 1 — Root Directory (/)

### Purpose

The root directory is the top-level directory of Linux. Every file and directory originates from here.

### Command

```bash
ls -l /
```

### Observation

Observed important directories such as:

```bash
bin
boot
dev
etc
home
usr
var
tmp
```

### I would use this when

I need to navigate the Linux file system or locate important system directories.

---

## Directory 2 — Home Directory (/home)

### Purpose

Stores personal directories and files for regular users.

### Command

```bash
ls -l /home
```

### Observation

Displayed user directories.

Example:

```bash
shubham
ubuntu
```

### I would use this when

Managing user files, scripts, projects, and personal configurations.

---

## Directory 3 — Root User Home (/root)

### Purpose

Home directory of the root (administrator) user.

### Command

```bash
sudo ls -l /root
```

### Observation

Contains root user configuration files and administrative data.

### I would use this when

Performing administrative tasks requiring root privileges.

---

## Directory 4 — Configuration Directory (/etc)

### Purpose

Contains system-wide configuration files.

### Command

```bash
ls -l /etc
```

### Observation

Observed files and directories such as:

```bash
hostname
hosts
passwd
ssh
```

### I would use this when

Configuring services, networking, users, and system settings.

---

## Directory 5 — Log Directory (/var/log)

### Purpose

Stores application and system log files.

### Command

```bash
ls -l /var/log
```

### Observation

Observed log files such as:

```bash
syslog
auth.log
kern.log
```

### I would use this when

Troubleshooting service failures, authentication issues, and system errors.

---

## Directory 6 — Temporary Files (/tmp)

### Purpose

Stores temporary files created by applications and users.

### Command

```bash
ls -l /tmp
```

### Observation

Contains temporary directories and session files.

### I would use this when

Testing scripts and storing short-lived data.

---

## Directory 7 — Essential Binaries (/bin)

### Purpose

Contains essential Linux commands.

### Command

```bash
ls -l /bin
```

### Observation

Examples:

```bash
ls
cp
mv
cat
```

### I would use this when

Running fundamental Linux commands required for system operation.

---

## Directory 8 — User Commands (/usr/bin)

### Purpose

Contains user-level commands and utilities.

### Command

```bash
ls -l /usr/bin
```

### Observation

Examples:

```bash
grep
vim
nano
python3
```

### I would use this when

Running utilities and applications.

---

## Directory 9 — Optional Applications (/opt)

### Purpose

Stores third-party and optional software packages.

### Command

```bash
ls -l /opt
```

### Observation

Contains manually installed software packages.

### I would use this when

Installing custom enterprise software.

---

# Hands-On Practice

## Task 1 — Find Largest Log Files

### Command

```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```

### Observation

Displayed the largest log files consuming disk space.

### Learning

Useful when troubleshooting disk space issues caused by log growth.

---

## Task 2 — Check System Hostname

### Command

```bash
cat /etc/hostname
```

### Observation

Displayed the hostname of the Ubuntu system.

### Learning

Useful when identifying servers in multi-server environments.

---

## Task 3 — Check Home Directory

### Command

```bash
ls -la ~
```

### Observation

Displayed hidden and visible files.

Example:

```bash
.bashrc
.profile
Documents
Downloads
```

### Learning

Helpful for locating user-specific configuration files.

---

# Part 2 – Scenario-Based Practice

## Scenario 1 — Service Not Starting

### Problem

A web application service named **myapp** failed to start after a reboot.

### Step 1 — Check Service Status

```bash
systemctl status myapp
```

**Why:** Determines whether the service is active, failed, or stopped.

---

### Step 2 — Check Recent Logs

```bash
journalctl -u myapp -n 50
```

**Why:** Displays recent service logs to identify errors.

---

### Step 3 — Verify Startup Configuration

```bash
systemctl is-enabled myapp
```

**Why:** Checks whether the service is configured to start automatically.

---

### Step 4 — Verify Service Exists

```bash
systemctl list-units --type=service | grep myapp
```

**Why:** Confirms that systemd recognizes the service.

---

### What I Learned

Always start with service status, then investigate logs and startup settings.

---

## Scenario 2 — High CPU Usage

### Problem

The application server is responding slowly.

### Step 1 — Monitor CPU Usage

```bash
top
```

**Why:** Provides real-time CPU and memory usage.

---

### Step 2 — Interactive Monitoring

```bash
htop
```

**Why:** Easier visualization of running processes.

---

### Step 3 — Find Top CPU Consumers

```bash
ps aux --sort=-%cpu | head -10
```

**Why:** Displays processes consuming the most CPU.

---

### Step 4 — Investigate Process

```bash
ps -fp <PID>
```

**Why:** Displays detailed information about the process.

---

### What I Learned

Always identify the root cause before stopping any process.

---

## Scenario 3 — Finding Service Logs

### Problem

A developer wants to see Docker service logs.

### Step 1 — Check Service Status

```bash
systemctl status docker
```

**Why:** Verifies the service state.

---

### Step 2 — View Recent Logs

```bash
journalctl -u docker -n 50
```

**Why:** Displays the latest 50 log entries.

---

### Step 3 — Follow Logs in Real Time

```bash
journalctl -u docker -f
```

**Why:** Continuously monitors incoming logs.

---

### What I Learned

Systemd-managed services store logs in journald and can be viewed using journalctl.

---

## Scenario 4 — File Permission Issue

### Problem

A script named `backup.sh` cannot execute due to a permission denied error.

### Step 1 — Check Current Permissions

```bash
ls -l /home/user/backup.sh
```

**Why:** Displays file permissions.

---

### Step 2 — Add Execute Permission

```bash
chmod +x /home/user/backup.sh
```

**Why:** Makes the script executable.

---

### Step 3 — Verify Permissions

```bash
ls -l /home/user/backup.sh
```

**Before**

```bash
-rw-r--r--
```

**After**

```bash
-rwxr-xr-x
```

---

### Step 4 — Execute Script

```bash
./backup.sh
```

**Why:** Confirms the issue is resolved.

---

### What I Learned

Linux requires execute (`x`) permission before a script can run.

---

# Why This Matters for DevOps

Understanding the Linux file system helps with:

* Finding logs quickly during incidents
* Locating configuration files
* Understanding where applications store data
* Writing reliable automation scripts
* Troubleshooting production issues efficiently

Scenario-based troubleshooting helps prepare for:

* Real production incidents
* On-call support responsibilities
* DevOps interviews
* Infrastructure troubleshooting

---

# Summary

Today I practiced:

* Linux File System Hierarchy
* Understanding important system directories
* Finding logs and configuration files
* Service troubleshooting using systemctl
* Log analysis using journalctl
* CPU troubleshooting using top, htop, and ps
* File permission troubleshooting using chmod
* Real-world DevOps troubleshooting scenarios

This practice improved my understanding of Linux internals and strengthened my troubleshooting skills required for DevOps and production support environments.
