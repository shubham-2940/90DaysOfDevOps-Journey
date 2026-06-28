# Shell Scripting Cheat Sheet

> A quick reference guide for Bash/Shell scripting with short descriptions and practical examples.

---

# Table of Contents
1. Quick Reference Table
2. Basics
3. Operators and Conditionals
4. Loops
5. Functions
6. Text Processing Commands
7. Useful Patterns and One-Liners
8. Error Handling and Debugging

---

# 1. Quick Reference Table

| Topic | Key Syntax | Example |
|-------|------------|---------|
| Variable | `NAME="value"` | `NAME="DevOps"` |
| Argument | `$1`, `$2` | `./script.sh file.txt` |
| If | `if [ condition ]; then` | `if [ -f file ]; then` |
| For Loop | `for i in list; do` | `for i in 1 2 3; do` |
| Function | `name() {}` | `greet(){ echo "Hi"; }` |
| Grep | `grep pattern file` | `grep -i error app.log` |
| Awk | `awk '{print $1}' file` | `awk -F: '{print $1}' /etc/passwd` |
| Sed | `sed 's/old/new/g' file` | `sed -i 's/foo/bar/g' file` |

---

# 2. Basics

## Shebang (`#!/bin/bash`)
**Purpose:** Tells Linux to execute the script using the Bash shell.

```bash
#!/bin/bash
echo "Hello World"
```

## Running a Script
**Purpose:** Execute a shell script.

```bash
chmod +x script.sh
./script.sh
```

or

```bash
bash script.sh
```

## Comments
**Purpose:** Explain code for better readability.

```bash
# This is a comment
echo "Hello" # Inline comment
```

## Variables
**Purpose:** Store values for reuse.

```bash
NAME="Suraj"
echo $NAME
echo "$NAME"
echo '$NAME'
```

## Read User Input
**Purpose:** Accept input from the user.

```bash
read -p "Enter your name: " NAME
echo "Hello $NAME"
```

## Command-line Arguments
**Purpose:** Pass values while running the script.

| Variable | Description |
|----------|-------------|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Number of arguments |
| `$@` | All arguments |
| `$?` | Exit status of previous command |

```bash
echo "Script: $0"
echo "First: $1"
```

# 3. Operators and Conditionals

## String Comparison
**Purpose:** Compare text values.

```bash
[ "$a" = "$b" ]
[ "$a" != "$b" ]
[ -z "$a" ]
[ -n "$a" ]
```

## Integer Comparison
**Purpose:** Compare numeric values.

```bash
[ $a -eq $b ]
[ $a -ne $b ]
[ $a -lt $b ]
[ $a -gt $b ]
[ $a -le $b ]
[ $a -ge $b ]
```

## File Test Operators
**Purpose:** Check file properties.

| Operator | Description |
|----------|-------------|
| `-f` | Regular file |
| `-d` | Directory |
| `-e` | Exists |
| `-r` | Readable |
| `-w` | Writable |
| `-x` | Executable |
| `-s` | Not empty |

```bash
if [ -f test.txt ]; then
  echo "File exists"
fi
```

## if / elif / else
**Purpose:** Execute code based on conditions.

```bash
if [ $a -gt 10 ]; then
  echo "Greater"
elif [ $a -eq 10 ]; then
  echo "Equal"
else
  echo "Smaller"
fi
```

## Logical Operators
**Purpose:** Combine multiple conditions.

```bash
[ $a -gt 5 ] && echo OK
[ $a -lt 5 ] || echo FAIL
! grep "error" app.log
```

## Case Statement
**Purpose:** Match multiple values cleanly.

```bash
case $1 in
start) echo "Starting";;
stop) echo "Stopping";;
*) echo "Invalid";;
esac
```

# 4. Loops

## for Loop
**Purpose:** Repeat commands over a list.

```bash
for i in 1 2 3
do
 echo $i
done
```

### C-style

```bash
for ((i=1;i<=5;i++))
do
 echo $i
done
```

## while Loop
**Purpose:** Repeat while a condition is true.

```bash
count=1
while [ $count -le 5 ]
do
 echo $count
 ((count++))
done
```

## until Loop
**Purpose:** Repeat until a condition becomes true.

```bash
count=1
until [ $count -gt 5 ]
do
 echo $count
 ((count++))
done
```

## break / continue
**Purpose:** Control loop execution.

```bash
break
continue
```

## Loop Through Files
**Purpose:** Process multiple files.

```bash
for file in *.log
do
 echo "$file"
done
```

## Read File Line by Line
**Purpose:** Process text files safely.

```bash
while read line
do
 echo "$line"
done < file.txt
```

# 5. Functions

## Function
**Purpose:** Reuse a block of code.

```bash
greet() {
 echo "Hello"
}
greet
```

## Function Arguments
**Purpose:** Pass values into functions.

```bash
sum() {
 echo $(($1+$2))
}
sum 10 20
```

## Return vs Echo
**Purpose:** `return` sends an exit status, while `echo` outputs data.

```bash
hello(){
 echo "Welcome"
 return 0
}
```

## Local Variables
**Purpose:** Limit variable scope to the function.

```bash
demo(){
 local name="Linux"
 echo "$name"
}
```

# 6. Text Processing Commands

## grep
**Purpose:** Search text or patterns in files.

```bash
grep -i "error" app.log
grep -n "error" app.log
grep -c "error" app.log
grep -v "info" app.log
grep -r "TODO" .
grep -E "error|warning" app.log
```

## awk
**Purpose:** Extract and process structured text.

```bash
awk '{print $1}' file.txt
awk -F: '{print $1}' /etc/passwd
awk 'BEGIN{print "Start"}'
awk 'END{print "Done"}'
```

## sed
**Purpose:** Search, replace, insert, or delete text.

```bash
sed 's/foo/bar/' file.txt
sed -i 's/foo/bar/g' file.txt
sed '2d' file.txt
```

## cut
**Purpose:** Extract specific columns.

```bash
cut -d: -f1 /etc/passwd
```

## sort
**Purpose:** Sort text alphabetically or numerically.

```bash
sort file.txt
sort -n numbers.txt
sort -r file.txt
sort -u file.txt
```

## uniq
**Purpose:** Remove or count duplicate lines.

```bash
uniq file.txt
uniq -c file.txt
```

## tr
**Purpose:** Translate or delete characters.

```bash
echo "linux" | tr a-z A-Z
```

## wc
**Purpose:** Count lines, words, and characters.

```bash
wc file.txt
wc -l file.txt
wc -w file.txt
```

## head
**Purpose:** Display the first lines of a file.

```bash
head -5 file.txt
```

## tail
**Purpose:** Display the last lines or monitor logs.

```bash
tail -10 file.txt
tail -f app.log
```

# 7. Useful One-Liners

**Delete files older than 7 days**

```bash
find /var/log -name "*.log" -mtime +7 -delete
```

**Count lines in all log files**

```bash
wc -l *.log
```

**Replace text in multiple files**

```bash
sed -i 's/old/new/g' *.txt
```

**Check if nginx is running**

```bash
ps -ef | grep nginx
```

**Monitor disk usage**

```bash
df -h
```

**Tail logs and filter errors**

```bash
tail -f app.log | grep ERROR
```

# 8. Error Handling and Debugging

## Exit Codes
**Purpose:** Indicate success or failure.

```bash
echo $?
exit 0
exit 1
```

## set -e
**Purpose:** Exit immediately if a command fails.

```bash
set -e
```

## set -u
**Purpose:** Treat unset variables as errors.

```bash
set -u
```

## set -o pipefail
**Purpose:** Detect failures in pipelines.

```bash
set -o pipefail
```

## set -x
**Purpose:** Print each command before executing it.

```bash
set -x
```

## trap
**Purpose:** Run cleanup code before script exits.

```bash
cleanup(){
 echo "Cleaning up..."
}

trap cleanup EXIT
```
