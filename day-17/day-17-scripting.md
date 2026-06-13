# Day 17 – Shell Scripting: Loops, Arguments & Error Handling

## Objective

Learn how to:

- Use `for` and `while` loops in Bash
- Work with command-line arguments
- Automate package installation using scripts
- Implement basic error handling
- Validate root user permissions

---

# Task 1: For Loop

## Script 1: for_loop.sh

### Code

```bash
#!/bin/bash

FRUITS=("Mango" "Apple" "Banana" "Guava" "Pineapple")

for FRUIT in "${FRUITS[@]}"; do
    echo "Fruit: $FRUIT"
done
```

### Output

```text
Fruit: Mango
Fruit: Apple
Fruit: Banana
Fruit: Guava
Fruit: Pineapple
```

---

## Script 2: count.sh

### Code

```bash
#!/bin/bash

for NUM in {1..10}; do
    echo "$NUM"
done
```

### Output

```text
1
2
3
4
5
6
7
8
9
10
```

---

# Task 2: While Loop

## Script: countdown.sh

### Code

```bash
#!/bin/bash

read -p "Enter a number for countdown: " COUNT

while [[ $COUNT -ge 0 ]]; do
    echo "Countdown: $COUNT"
    ((COUNT--))
done

echo "Done!"
```

### Output

```text
Enter a number for countdown: 5

Countdown: 5
Countdown: 4
Countdown: 3
Countdown: 2
Countdown: 1
Countdown: 0
Done!
```

---

# Task 3: Command-Line Arguments

## Script 1: greet.sh

### Code

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Usage: ./greet.sh <name>"
else
    echo "Hello, $1!"
fi
```

### Output

#### With Argument

```bash
./greet.sh Suraj
```

```text
Hello, Suraj!
```

#### Without Argument

```bash
./greet.sh
```

```text
Usage: ./greet.sh <name>
```

---

## Script 2: args_demo.sh

### Code

```bash
#!/bin/bash

echo "Script Name: $0"
echo "Total Number of Arguments: $#"
echo "All Arguments: $@"
```

### Execution

```bash
./args_demo.sh Apple Banana Mango
```

### Output

```text
Script Name: ./args_demo.sh
Total Number of Arguments: 3
All Arguments: Apple Banana Mango
```

---

# Task 4: Install Packages via Script

## Script: install_packages.sh

### Code

```bash
#!/bin/bash

# Check if script is run as root
if [ "$EUID" -ne 0 ]; then
    echo "Run as root"
    exit 1
fi

PACKAGES=("nginx" "curl" "wget")

for PKG in "${PACKAGES[@]}"; do

    echo "Checking package: $PKG"

    if dpkg -s "$PKG" &> /dev/null; then
        echo "$PKG is already installed. Skipping..."
    else
        echo "$PKG is not installed. Installing..."

        apt update -y
        apt install -y "$PKG"

        if dpkg -s "$PKG" &> /dev/null; then
            echo "$PKG installed successfully."
        else
            echo "Failed to install $PKG."
        fi
    fi

    echo "-----------------------------------"

done
```

### Run Script

```bash
sudo ./install_packages.sh
```

### Sample Output

```text
Checking package: nginx
nginx is already installed. Skipping...

Checking package: curl
curl is already installed. Skipping...

Checking package: wget
wget installed successfully.
```

---

# Task 5: Error Handling

## Script: safe_script.sh

### Code

```bash
#!/bin/bash

set -e

mkdir /tmp/devops-test || {
    echo "Failed to create directory"
    exit 1
}

cd /tmp/devops-test || {
    echo "Failed to navigate to directory"
    exit 1
}

touch test.txt || {
    echo "Failed to create file"
    exit 1
}

echo "Script completed successfully."
```

### Output

```text
Script completed successfully.
```

### Example Error Handling

```bash
mkdir /tmp/devops-test || echo "Directory already exists"
```

Output:

```text
Directory already exists
```

---

# Commands Used

### Make Scripts Executable

```bash
chmod +x for_loop.sh
chmod +x count.sh
chmod +x countdown.sh
chmod +x greet.sh
chmod +x args_demo.sh
chmod +x install_packages.sh
chmod +x safe_script.sh
```

### Execute Scripts

```bash
./for_loop.sh
./count.sh
./countdown.sh
./greet.sh Suraj
./args_demo.sh one two three
sudo ./install_packages.sh
./safe_script.sh
```

---

# Key Concepts Learned

## 1. Loops in Bash

- `for` loops are used to iterate through a list of values.
- `while` loops execute repeatedly until a condition becomes false.
- Useful for automation and repetitive tasks.

## 2. Command-Line Arguments

- `$0` → Script name
- `$1` → First argument
- `$#` → Number of arguments
- `$@` → All arguments

These allow scripts to accept user input dynamically.

## 3. Error Handling and Script Safety

- `set -e` stops script execution when an error occurs.
- `||` operator helps display custom error messages.
- Checking root privileges prevents unauthorized package installations.
- Proper error handling makes scripts more reliable and production-ready.

---

# Summary

On Day 17, I practiced Bash scripting fundamentals by implementing loops, handling command-line arguments, automating package installation, validating root permissions, and adding error handling. These concepts are essential for DevOps engineers because they help automate repetitive tasks and improve system administration efficiency.