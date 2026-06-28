# Day 19 – Shell Scripting Project: Log Rotation, Backup & Crontab

## Objective

The goal of Day 19 is to apply the shell scripting concepts learned in the previous days by creating real-world automation scripts. These scripts perform common system administration tasks such as log rotation, server backup, scheduled maintenance, and task automation using cron jobs.

---

# Task 1: Log Rotation Script

## Objective

Create a shell script that:

* Accepts a log directory as an argument.
* Compresses `.log` files older than 7 days.
* Deletes compressed `.gz` files older than 30 days.
* Displays the number of files compressed and deleted.
* Exits with an error if the directory does not exist.

---

## Script: `log_rotate.sh`

```bash
#!/bin/bash

# Check if log directory is provided
if [ $# -ne 1 ]; then
    echo "Usage: $0 <log_directory>"
    exit 1
fi

LOG_DIR=$1

# Check if directory exists
if [ ! -d "$LOG_DIR" ]; then
    echo "Error: Directory does not exist."
    exit 1
fi

# Compress .log files older than 7 days
COMPRESSED=$(find "$LOG_DIR" -name "*.log" -mtime +7 | wc -l)
find "$LOG_DIR" -name "*.log" -mtime +7 -exec gzip {} \;

# Delete .gz files older than 30 days
DELETED=$(find "$LOG_DIR" -name "*.gz" -mtime +30 | wc -l)
find "$LOG_DIR" -name "*.gz" -mtime +30 -delete

# Display results
echo "Compressed files: $COMPRESSED"
echo "Deleted files: $DELETED"
```

---

## Make Script Executable

```bash
chmod +x log_rotate.sh
```

---

## Run the Script

```bash
./log_rotate.sh /var/log/myapp
```

---

## Sample Output

```text
Compressed files: 5
Deleted files: 2
```

---

# Task 2: Server Backup Script

## Objective

Create a shell script that:

* Accepts the source directory and backup destination as arguments.
* Creates a timestamped `.tar.gz` archive.
* Verifies successful backup creation.
* Displays archive name and size.
* Deletes backups older than 14 days.
* Exits if the source directory does not exist.

---

## Script: `backup.sh`

```bash
#!/bin/bash

# Check arguments
if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_directory> <backup_directory>"
    exit 1
fi

SOURCE=$1
DEST=$2

# Check if source exists
if [ ! -d "$SOURCE" ]; then
    echo "Error: Source directory does not exist."
    exit 1
fi

# Create backup directory
mkdir -p "$DEST"

# Create backup file
DATE=$(date +%Y-%m-%d)
BACKUP_FILE="$DEST/backup-$DATE.tar.gz"

# Create archive
tar -czf "$BACKUP_FILE" "$SOURCE"

# Verify backup
if [ -f "$BACKUP_FILE" ]; then
    echo "Backup created successfully!"
    echo "Archive: $(basename "$BACKUP_FILE")"
    echo "Size: $(du -h "$BACKUP_FILE" | cut -f1)"
else
    echo "Backup failed."
    exit 1
fi

# Delete backups older than 14 days
find "$DEST" -name "backup-*.tar.gz" -mtime +14 -delete

echo "Old backups (older than 14 days) deleted."
```

---

## Make Script Executable

```bash
chmod +x backup.sh
```

---

## Run the Script

```bash
./backup.sh /home/user/Documents /home/user/backups
```

---

## Sample Output

```text
Backup created successfully!
Archive: backup-2026-06-19.tar.gz
Size: 2.4M
Old backups (older than 14 days) deleted.
```

---

# Task 3: Crontab

## Check Existing Cron Jobs

```bash
crontab -l
```

If no cron jobs exist:

```text
no crontab for username
```

---

## Cron Syntax

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week (0-7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of Month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
```

---

## Cron Entries

### Run `log_rotate.sh` every day at **2:00 AM**

```cron
0 2 * * * /home/user/scripts/log_rotate.sh
```

---

### Run `backup.sh` every **Sunday at 3:00 AM**

```cron
0 3 * * 0 /home/user/scripts/backup.sh
```

---

### Run `health_check.sh` every **5 minutes**

```cron
*/5 * * * * /home/user/scripts/health_check.sh
```

> **Note:** These entries are written for documentation purposes and were not applied to the system.

---

# Task 4: Scheduled Maintenance Script

## Objective

Create a maintenance script that:

* Runs the log rotation script.
* Runs the backup script.
* Logs all output with timestamps.
* Can be scheduled using cron.

---

## Script: `maintenance.sh`

```bash
#!/bin/bash

LOG_FILE="/var/log/maintenance.log"

echo "===== $(date) =====" >> "$LOG_FILE"

./log_rotate.sh /var/log/myapp >> "$LOG_FILE" 2>&1
./backup.sh /home/user/Documents /home/user/backups >> "$LOG_FILE" 2>&1

echo "Maintenance completed." >> "$LOG_FILE"
```

> **Note:** While testing as a normal user, you can replace:

```bash
LOG_FILE="/var/log/maintenance.log"
```

with

```bash
LOG_FILE="$HOME/maintenance.log"
```

to avoid permission issues.

---

## Make Script Executable

```bash
chmod +x maintenance.sh
```

---

## Run the Script

```bash
./maintenance.sh
```

---

## Sample Log Output

```text
===== Thu Jun 19 10:30:15 IST 2026 =====
Compressed files: 3
Deleted files: 1
Backup created successfully!
Archive: backup-2026-06-19.tar.gz
Size: 2.4M
Old backups (older than 14 days) deleted.
Maintenance completed.
```

---

## Cron Entry for Maintenance Script

Run every day at **1:00 AM**.

```cron
0 1 * * * /home/user/scripts/maintenance.sh
```

---

# Commands Used

```bash
chmod +x log_rotate.sh
chmod +x backup.sh
chmod +x maintenance.sh

./log_rotate.sh /var/log/myapp

./backup.sh /home/user/Documents /home/user/backups

./maintenance.sh

crontab -l
```

---

# Files Created

```text
day-19-project.md
log_rotate.sh
backup.sh
maintenance.sh
```

---

# Key Learnings

### 1. Automation Saves Time

Shell scripts can automate repetitive administrative tasks like backups, log cleanup, and maintenance, reducing manual effort and improving efficiency.

### 2. Cron Enables Scheduled Execution

Cron allows scripts to run automatically at specified times, making it an essential tool for Linux system administration and DevOps automation.

### 3. Logging and Error Handling Improve Reliability

Adding proper validation, timestamps, and logging helps monitor script execution, troubleshoot failures, and build reliable automation workflows.

---

# Conclusion

Day 19 focused on building practical automation solutions using Bash scripting. By creating log rotation, backup, maintenance, and cron scheduling scripts, I gained hands-on experience with real-world Linux administration tasks that are commonly performed by DevOps and System Engineers.

These mini-projects strengthened my understanding of shell scripting, task scheduling, file management, and automation—key skills required for modern DevOps practices.
