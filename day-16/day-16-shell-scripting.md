# Day 16 – Shell Scripting Basics

## Objective

Learn the fundamentals of Bash scripting, including script execution, variables, user input, conditional statements, file checks, and basic service management.

---

# Task 1: Your First Script

## hello.sh

```bash
#!/bin/bash

echo "Hello, DevOps!"
```

### Make Script Executable

```bash
chmod +x hello.sh
```

### Run Script

```bash
./hello.sh
```

### Output

```text
Hello, DevOps!
```

### What Happens If the Shebang Line Is Removed?

The shebang (`#!/bin/bash`) tells Linux which interpreter should execute the script.

Without the shebang:

```bash
echo "Hello, DevOps!"
```

The script may still work when executed from a Bash shell, but it becomes interpreter-dependent. On different systems or shells, the script may fail or behave unexpectedly.

Benefits of using a shebang:

* Specifies the interpreter explicitly.
* Improves portability.
* Ensures consistent execution.

---

# Task 2: Variables

## variables.sh

```bash
#!/bin/bash

NAME="Shubham"
ROLE="DevOps Engineer"

echo "Hello, I am $NAME and I am a $ROLE"
```

### Output

```text
Hello, I am Shubham and I am a DevOps Engineer
```

## Single Quotes vs Double Quotes

### Double Quotes

```bash
echo "Hello $NAME"
```

Output:

```text
Hello Shubham
```

Variables are expanded inside double quotes.

### Single Quotes

```bash
echo 'Hello $NAME'
```

Output:

```text
Hello $NAME
```

Variables are not expanded inside single quotes.

### Learning

* Double quotes allow variable expansion.
* Single quotes treat everything literally.
* Variables should not contain spaces around `=`.

---

# Task 3: User Input Using read

## greet.sh

```bash
#!/bin/bash

read -p "Enter your name: " NAME
read -p "Enter your favourite tool: " TOOL

echo "Hello $NAME, your favourite tool is $TOOL"
```

### Sample Output

```text
Enter your name: Shubham
Enter your favourite tool: Docker

Hello Shubham, your favourite tool is Docker
```

### Learning

* `read` accepts user input.
* `-p` displays a prompt message.
* User input can be stored in variables and reused.

---

# Task 4: If-Else Conditions

## check_number.sh

### Objective

Determine whether a number is positive, negative, or zero.

### Script

```bash
#!/bin/bash

read -p "Enter a number: " number

if (( number > 0 )); then
    echo "Number is positive"
elif (( number < 0 )); then
    echo "Number is negative"
else
    echo "Number is zero"
fi
```

### Sample Output

```text
Enter a number: 10
Number is positive
```

```text
Enter a number: -5
Number is negative
```

```text
Enter a number: 0
Number is zero
```

---

## file_check.sh

### Objective

Check whether a file exists.

### Script

```bash
#!/bin/bash

read -p "Enter file name: " filename

if [ -f "$filename" ]; then
    echo "File exists."
    echo "File path: $(realpath "$filename")"
else
    echo "File does not exist."
fi
```

### Sample Output

```text
Enter file name: notes.txt

File exists.
File path: /home/ubuntu/scripts-practice/notes.txt
```

### Learning

* `-f` checks whether a file exists.
* `realpath` displays the absolute path.
* Quotes prevent issues with filenames containing spaces.

---

# Task 5: Service Status Checker

## server_check.sh

### Objective

Check the status of a Linux service and provide user-friendly output.

### Script

```bash
#!/bin/bash

read -p "Enter the service name: " SERVICE
read -p "Do you want to check the status of service? (y/n): " choice

if [ "$choice" != "y" ]; then
    echo "Skipped."
    exit 0
fi

if systemctl list-unit-files | grep -q "^${SERVICE}.service"; then

    if systemctl is-active --quiet "$SERVICE"; then
        echo "✅ Service '$SERVICE' is ACTIVE."
    else
        echo "❌ Service '$SERVICE' is INACTIVE."

        read -p "Do you want to start this service? (y/n): " start_choice

        if [ "$start_choice" = "y" ]; then
            sudo systemctl start "$SERVICE"

            if systemctl is-active --quiet "$SERVICE"; then
                echo "✅ Service started successfully."
            else
                echo "❌ Failed to start service."
            fi
        fi
    fi

    echo ""
    echo "===== Detailed Service Status ====="
    systemctl status "$SERVICE" --no-pager

else
    echo "❌ Service '$SERVICE' not found."

    read -p "Do you want to install it? (y/n): " install_choice

    if [ "$install_choice" = "y" ]; then
        sudo apt update
        sudo apt install -y "$SERVICE"

        echo ""
        echo "===== Service Status ====="
        systemctl status "$SERVICE" --no-pager
    else
        echo "Installation skipped."
    fi
fi
```

### Sample Output

```text
Enter the service name: ssh
Do you want to check the status of service? (y/n): y

✅ Service 'ssh' is ACTIVE.
```

### Additional Functionality Practiced

* Verified service status using `systemctl is-active`.
* Displayed detailed service information using `systemctl status --no-pager`.
* Started inactive services.
* Prompted user to install missing services.
* Installed services using `apt install`.
* Used nested if-else conditions.

---

# Commands Practiced

## Make a Script Executable

```bash
chmod +x script.sh
```

## Execute a Script

```bash
./script.sh
```

## Check Service Status

```bash
systemctl status ssh
```

## Check Whether a Service Is Active

```bash
systemctl is-active ssh
```

## Start a Service

```bash
sudo systemctl start ssh
```

## Install a Package

```bash
sudo apt install nginx
```

---

# Key Learnings

### 1. Bash Scripts Automate Repetitive Tasks

Shell scripts allow administrators and DevOps engineers to automate system operations and reduce manual effort.

### 2. Variables and User Input Make Scripts Dynamic

Using variables and the `read` command allows scripts to accept user input and behave differently based on user choices.

### 3. Conditional Statements Enable Decision-Making

The `if`, `elif`, and `else` statements help scripts react to different situations such as checking files, validating input, and monitoring services.

---

# Summary

Day 16 focused on the fundamentals of Bash Shell Scripting. I created multiple scripts to understand script execution, variables, user input, conditional logic, file validation, and Linux service management. By the end of the day, I was able to build an interactive service-monitoring script that checks service status, starts inactive services, and optionally installs missing services.
