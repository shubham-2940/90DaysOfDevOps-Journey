# Day 20 – Bash Scripting Challenge: Log Analyzer and Report Generator

## Objective

The objective of this project is to create a Bash script that analyzes a log file, extracts useful information, and generates a summary report. The script also archives the processed log file after analysis.

---

# Script: `log_analyzer.sh`

```bash
#!/bin/bash

# Check if log file is provided
if [ $# -eq 0 ]; then
    echo "Usage: ./log_analyzer.sh <log_file>"
    exit 1
fi

logfile=$1

# Check if file exists
if [ ! -f "$logfile" ]; then
    echo "Error: File does not exist."
    exit 1
fi

report="log_report_$(date +%Y-%m-%d).txt"

echo "----- Log Analysis -----"

# Total lines
total_lines=$(wc -l < "$logfile")

# Count ERROR and Failed
error_count=$(grep -Ei "ERROR|Failed" "$logfile" | wc -l)

echo "Total Errors: $error_count"

echo
echo "--- Critical Events ---"
critical=$(grep -n "CRITICAL" "$logfile")

if [ -z "$critical" ]; then
    echo "No critical events found."
else
    echo "$critical"
fi

echo
echo "--- Top 5 Error Messages ---"

top_errors=$(grep "ERROR" "$logfile" | cut -d']' -f2 | sort | uniq -c | sort -rn | head -5)

echo "$top_errors"

# Generate report
{
echo "Log Analysis Report"
echo "==========================="
echo "Date: $(date)"
echo "Log File: $logfile"
echo "Total Lines: $total_lines"
echo "Total Errors: $error_count"

echo
echo "Top 5 Error Messages"
echo "--------------------"
echo "$top_errors"

echo
echo "Critical Events"
echo "---------------"
if [ -z "$critical" ]; then
    echo "No critical events found."
else
    echo "$critical"
fi
} > "$report"

# Optional Archive
mkdir -p archive
mv "$logfile" archive/

echo
echo "Report generated: $report"
echo "Log file moved to archive/"
```

---

# How to Run

Give execute permission to the script:

```bash
chmod +x log_analyzer.sh
```

Run the script by providing the log file as an argument:

```bash
./log_analyzer.sh sample_log.log
```

---

# Sample Console Output

```text
----- Log Analysis -----

Total Errors: 18

--- Critical Events ---
15:2026-06-22 11:15:02 [CRITICAL] Disk full
39:2026-06-22 11:20:51 [CRITICAL] Out of memory

--- Top 5 Error Messages ---

6 Failed to connect
5 Disk full
3 Invalid input
2 Out of memory
2 Segmentation fault

Report generated: log_report_2026-06-22.txt
Log file moved to archive/
```

---

# Sample Report (`log_report_2026-06-22.txt`)

```text
Log Analysis Report
===========================
Date: Mon Jun 22 20:30:15 IST 2026
Log File: sample_log.log
Total Lines: 100
Total Errors: 18

Top 5 Error Messages
--------------------
6 Failed to connect
5 Disk full
3 Invalid input
2 Out of memory
2 Segmentation fault

Critical Events
---------------
15:2026-06-22 11:15:02 [CRITICAL] Disk full
39:2026-06-22 11:20:51 [CRITICAL] Out of memory
```

---

# Commands Used

| Command    | Purpose                                               |
| ---------- | ----------------------------------------------------- |
| `grep`     | Search for ERROR, Failed, and CRITICAL messages       |
| `grep -n`  | Display matching lines with line numbers              |
| `grep -Ei` | Perform case-insensitive search for multiple keywords |
| `wc -l`    | Count total lines and errors                          |
| `cut`      | Extract only the error message                        |
| `sort`     | Sort error messages                                   |
| `uniq -c`  | Count duplicate messages                              |
| `head -5`  | Display top five results                              |
| `date`     | Generate report filename with current date            |
| `mkdir -p` | Create archive directory if it doesn't exist          |
| `mv`       | Move processed log file to archive                    |

---

# Features Implemented

✅ Accepts log file path as a command-line argument.

✅ Validates that the argument is provided.

✅ Checks whether the log file exists.

✅ Counts all lines containing **ERROR** or **Failed**.

✅ Displays all **CRITICAL** events with line numbers.

✅ Finds and displays the top five most common error messages.

✅ Generates a summary report named:

```text
log_report_<date>.txt
```

✅ Creates an **archive/** directory automatically.

✅ Moves the processed log file into the archive folder.

---

# What I Learned

1. How to validate command-line arguments and verify file existence in Bash.
2. How to analyze log files efficiently using commands like `grep`, `sort`, `uniq`, `cut`, and `wc`.
3. How to generate reports automatically and organize processed log files using Bash scripting.

---

# Conclusion

This project demonstrates how Bash scripting can automate log analysis by counting errors, identifying critical events, generating summary reports, and archiving processed log files. It provides a simple and practical example of using shell scripting for real-world system administration tasks.
