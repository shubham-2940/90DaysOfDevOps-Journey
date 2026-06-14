# Day 18 – Shell Scripting: Functions & Intermediate Concepts

## Objective

On Day 18, I learned how to write cleaner and reusable Bash scripts using functions. I also explored Bash Strict Mode (`set -euo pipefail`), local variables, return values, and built a real-world System Information Reporter script by combining multiple functions.

---

# What I Practiced

* Created reusable functions
* Passed arguments to functions
* Used `set -euo pipefail` for safer scripting
* Worked with local and global variables
* Built an intermediate system monitoring script
* Organized scripts using a `main()` function

---

# Task 1: Basic Functions

## Script: `functions.sh`

### Code

```bash
#!/bin/bash

# Function to greet user
greet() {
    echo "Hello, $1!"
}

# Function to add two numbers
add() {
    sum=$(($1 + $2))
    echo "Sum of $1 and $2 is: $sum"
}

# Calling functions
greet "Suraj"
add 10 20
```

### Output

```
Hello, Suraj!
Sum of 10 and 20 is: 30
```

### What I Learned

* Functions improve code reusability.
* Arguments are passed while calling functions.
* Function parameters are accessed using `$1`, `$2`, etc.

---

# Task 2: Functions with Return Values

## Script: `disk_check.sh`

### Code

```bash
#!/bin/bash

check_disk() {
    echo "========================================"
    echo "          DISK USAGE REPORT"
    echo "========================================"
    df -h /
    echo
}

check_memory() {
    echo "========================================"
    echo "         MEMORY USAGE REPORT"
    echo "========================================"
    free -h
    echo
}

echo "System Health Check"
echo "Date: $(date)"
echo

check_disk
check_memory
```

### Output

```
System Health Check
Date: Sat Jun 14 2026

========================================
          DISK USAGE REPORT
========================================

Filesystem      Size Used Avail Use% Mounted on
/dev/root        14G 2.7G 11G 20% /

========================================
         MEMORY USAGE REPORT
========================================

               total        used        free
Mem:           7.6Gi       489Mi       7.0Gi
Swap:             0B          0B          0B
```

### What I Learned

* Functions can be reused multiple times.
* Commands like `df` and `free` can be organized into separate functions.
* Proper formatting makes script output easier to read.

---

# Task 3: Bash Strict Mode (`set -euo pipefail`)

## Script: `strict_demo.sh`

### Code

```bash
#!/bin/bash

set -euo pipefail

echo "===== Bash Strict Mode Demo ====="

echo
echo "Testing set -u"
echo "User: $username"

echo
echo "Testing set -e"
ls missing_directory

echo
echo "Testing pipefail"
cat missing.txt | grep hello
```

### Output

```
===== Bash Strict Mode Demo =====

Testing set -u

./strict_demo.sh: line 8: username: unbound variable
```

(After commenting the undefined variable)

```
Testing set -e

ls: cannot access 'missing_directory': No such file or directory
```

(After commenting the failed command)

```
Testing pipefail

cat: missing.txt: No such file or directory
```

---

## Explanation of `set -euo pipefail`

### `set -e`

* Exits the script immediately if any command fails.
* Prevents execution of remaining commands after an error.

Example:

```bash
ls missing_directory
echo "This line will never execute"
```

---

### `set -u`

* Treats undefined variables as errors.
* Helps detect typos and uninitialized variables.

Example:

```bash
echo "$username"
```

If `username` is not defined, the script exits.

---

### `set -o pipefail`

* Makes a pipeline fail if any command in the pipeline fails.
* Prevents hidden failures inside piped commands.

Example:

```bash
cat missing.txt | grep hello
```

If `cat` fails, the entire pipeline fails.

---

## Why Strict Mode?

Using

```bash
set -euo pipefail
```

makes Bash scripts safer, more reliable, and easier to debug. It is considered a best practice in DevOps scripting.

---

# Task 4: Local Variables

## Script: `local_demo.sh`

### Code

```bash
#!/bin/bash

local_function() {
    local message="I am a LOCAL variable"
    echo "Inside local_function: $message"
}

global_function() {
    message="I am a GLOBAL variable"
    echo "Inside global_function: $message"
}

echo "===== Local Variable Demo ====="

local_function
echo "Outside local_function: $message"

echo

echo "===== Global Variable Demo ====="

global_function
echo "Outside global_function: $message"
```

### Output

```
===== Local Variable Demo =====

Inside local_function: I am a LOCAL variable
Outside local_function:

===== Global Variable Demo =====

Inside global_function: I am a GLOBAL variable
Outside global_function: I am a GLOBAL variable
```

### What I Learned

* Local variables exist only inside the function.
* Global variables remain accessible throughout the script.
* Using `local` helps avoid accidental variable modification.

---

# Task 5: Build a System Information Reporter

## Script: `system_info.sh`

### Features

The script contains separate functions for:

* Hostname and OS Information
* System Uptime
* Disk Usage
* Memory Usage
* Top 5 CPU-consuming Processes

A `main()` function calls all the above functions in sequence while displaying clean section headers.

### Sample Output

```
#########################################
       SYSTEM INFORMATION REPORT
#########################################

Hostname : ip-172-31-24-74
OS       : Ubuntu 24.04 LTS
Kernel   : 6.x.x

System Uptime
-------------
up 3 hours, 21 minutes

Disk Usage
----------
Filesystem      Size Used Avail Use%
/dev/root        14G 2.7G 11G 20%

Memory Usage
------------
Total : 7.6Gi
Used  : 489Mi
Free  : 7.0Gi

Top CPU Processes
-----------------
PID USER %CPU COMMAND
...
```

### What I Learned

* Large scripts become easier to maintain using functions.
* `main()` improves readability and execution flow.
* Well-formatted output makes monitoring scripts more useful.

---

# Important Bash Commands Used

| Command     | Description                    |
| ----------- | ------------------------------ |
| `hostname`  | Displays system hostname       |
| `uname -r`  | Displays kernel version        |
| `uptime -p` | Shows system uptime            |
| `df -h`     | Displays disk usage            |
| `free -h`   | Displays memory usage          |
| `ps`        | Displays running processes     |
| `head`      | Displays first few lines       |
| `sort`      | Sorts command output           |
| `grep`      | Searches for matching text     |
| `cut`       | Extracts specific fields       |
| `date`      | Displays current date and time |

---

# Best Practices Learned

* Always start Bash scripts with:

```bash
#!/bin/bash
```

* Enable Strict Mode:

```bash
set -euo pipefail
```

* Use functions to avoid duplicate code.
* Keep variables local whenever possible.
* Use meaningful function names.
* Add comments for better readability.
* Organize larger scripts using a `main()` function.

---

# Key Takeaways

### 1. Functions Improve Reusability

Functions help avoid repeating code and make scripts modular.

### 2. Strict Mode Makes Scripts Safer

Using `set -euo pipefail` prevents common scripting mistakes and makes debugging easier.

### 3. Local Variables Prevent Bugs

Using the `local` keyword keeps variables inside functions and avoids unintended changes to global variables.

---

# Conclusion

Day 18 focused on writing cleaner, safer, and more maintainable Bash scripts using functions, strict mode, local variables, and modular design. These concepts are essential for building reliable automation scripts in Linux and DevOps environments.
