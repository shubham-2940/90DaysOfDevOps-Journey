# Day 20 – Log Analyzer and Report Generator

## Objective

Create a Bash script that analyzes a log file and generates a summary report.

---

## Script

`log_analyzer.sh`

### Features

- Accepts log file as command-line argument
- Validates file existence
- Counts ERROR and Failed messages
- Displays CRITICAL events with line numbers
- Finds top 5 ERROR messages
- Generates a summary report
- Moves processed log file to archive/

---

## Commands Used

| Command | Purpose |
|---------|---------|
| grep | Search log entries |
| wc | Count lines |
| cut | Extract error message |
| sort | Sort output |
| uniq -c | Count duplicate messages |
| head | Display top 5 results |
| mkdir -p | Create archive folder |
| mv | Move processed log |
| date | Generate report filename |

---

## Sample Output

```text
Total Errors: 18

--- Critical Events ---
15:2026-06-22 11:15:02 [CRITICAL] Disk full

--- Top 5 Error Messages ---

6 Failed to connect
5 Disk full
3 Invalid input
2 Out of memory
```

---

## Report Generated

```
log_report_2026-06-22.txt
```

---

## What I Learned

1. How to search log files using `grep`.
2. How to count duplicate messages using `sort` and `uniq`.
3. How to generate reports and organize processed log files using Bash.
