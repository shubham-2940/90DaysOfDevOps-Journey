# Shell Scripting Cheat Sheet

> A practical Bash reference guide for DevOps Engineers.

---

# Quick Reference

| Topic | Syntax | Example |
|---|---|---|
| Variable | `VAR="value"` | `NAME="DevOps"` |
| Argument | `$1` | `./script.sh file.txt` |
| If | `if [ condition ]; then` | `if [ -f file ]; then` |
| For | `for i in list; do` | `for i in 1 2 3; do` |
| Function | `name(){}` | `greet(){ echo Hi; }` |
| Grep | `grep pattern file` | `grep -i error app.log` |
| Awk | `awk '{print $1}'` | `awk -F: '{print $1}' /etc/passwd` |
| Sed | `sed 's/a/b/g'` | `sed -i 's/foo/bar/g' file` |

---

# Table of Contents

1. Basics
2. Variables & Input
3. Arguments
4. Operators
5. Conditionals
6. Loops
7. Functions
8. Text Processing
9. One-Liners
10. Error Handling
11. Best Practices

---

# 1. Basics

## Shebang

```bash
#!/bin/bash
```

Specifies Bash interpreter.

## Run Script

```bash
chmod +x script.sh
./script.sh
```

or

```bash
bash script.sh
```

## Comments

```bash
# Single-line comment

echo "Hello" # Inline comment
```

---

# Variables

```bash
NAME="Suraj"
echo $NAME
echo "$NAME"
echo '$NAME'
```

Double quotes expand variables; single quotes treat text literally.

## Read Input

```bash
read NAME
echo "Hello $NAME"
```

---

# Command-line Arguments

```bash
echo $0
echo $1
echo $2
echo $#
echo $@
echo $?
```

| Symbol | Meaning |
|---|---|
|$0|Script name|
|$1|First argument|
|$2|Second argument|
|$#|Argument count|
|$@|All arguments|
|$?|Exit status|

---

# 2. Operators

## String

```bash
[ "$A" = "$B" ]
[ "$A" != "$B" ]
[ -z "$A" ]
[ -n "$A" ]
```

## Integer

```bash
[ $A -eq $B ]
[ $A -ne $B ]
[ $A -lt $B ]
[ $A -gt $B ]
[ $A -le $B ]
[ $A -ge $B ]
```

## File Tests

```bash
-f
-d
-e
-r
-w
-x
-s
```

Example

```bash
if [ -f test.txt ]; then
 echo Exists
fi
```

## Logical Operators

```bash
&&
||
!
```

---

# 3. Conditionals

```bash
if [ $AGE -ge 18 ]; then
 echo Adult
elif [ $AGE -gt 12 ]; then
 echo Teen
else
 echo Child
fi
```

## Case

```bash
case $1 in
start) echo Starting;;
stop) echo Stopping;;
restart) echo Restart;;
*) echo Invalid;;
esac
```

---

# 4. Loops

## For

```bash
for i in 1 2 3
do
 echo $i
done
```

### C Style

```bash
for((i=1;i<=5;i++))
do
 echo $i
done
```

## While

```bash
COUNT=1
while [ $COUNT -le 5 ]
do
 echo $COUNT
 COUNT=$((COUNT+1))
done
```

## Until

```bash
COUNT=1
until [ $COUNT -gt 5 ]
do
 echo $COUNT
 COUNT=$((COUNT+1))
done
```

## break / continue

```bash
break
continue
```

## Loop Files

```bash
for file in *.log
do
 echo $file
done
```

## Read File

```bash
while read line
do
 echo "$line"
done < file.txt
```

---

# 5. Functions

```bash
greet(){
 local NAME=$1
 echo "Hello $NAME"
}

greet Suraj
```

Return

```bash
sum(){
 echo $(($1+$2))
}

RESULT=$(sum 5 10)
```

---

# 6. Text Processing

## grep

```bash
grep error app.log
grep -i error app.log
grep -r error .
grep -n error app.log
grep -c error app.log
grep -v info app.log
grep -E "error|warning" app.log
```

## awk

```bash
awk '{print $1}' file
awk -F: '{print $1}' /etc/passwd
awk 'NR>1'
awk 'BEGIN{print "Start"}'
awk 'END{print "Done"}'
```

## sed

```bash
sed 's/foo/bar/' file
sed 's/foo/bar/g' file
sed -i 's/foo/bar/g' file
sed '2d' file
```

## cut

```bash
cut -d, -f1 file.csv
```

## sort

```bash
sort file
sort -n file
sort -r file
sort -u file
```

## uniq

```bash
uniq file
uniq -c file
```

## tr

```bash
tr a-z A-Z
tr -d '\r'
```

## wc

```bash
wc file
wc -l file
wc -w file
wc -c file
```

## head/tail

```bash
head -10 file
tail -20 file
tail -f app.log
```

---

# 7. Useful One-Liners

```bash
find . -name "*.log" -mtime +7 -delete
```

```bash
wc -l *.log
```

```bash
find . -type f -exec sed -i 's/http/https/g' {} +
```

```bash
systemctl status nginx
```

```bash
df -h | awk '$5+0>80'
```

```bash
tail -f app.log | grep ERROR
```

```bash
ps -ef | grep java
```

```bash
du -sh *
```

---

# 8. Error Handling

Exit status

```bash
echo $?
exit 0
exit 1
```

Strict Mode

```bash
set -e
set -u
set -o pipefail
set -x
```

Recommended

```bash
#!/bin/bash
set -euo pipefail
```

Trap

```bash
cleanup(){
 echo Cleaning
}

trap cleanup EXIT
```

---

# 9. Best Practices

- Always use `#!/bin/bash`
- Use `set -euo pipefail`
- Quote variables
- Use functions
- Validate inputs
- Use meaningful names
- Check exit codes
- Add comments only where necessary
- Test with ShellCheck
- Keep scripts modular

---

# 10. Common Interview Questions

### Difference between `$@` and `$*`

- `$@` preserves individual arguments.
- `$*` combines them into a single string.

### Difference between `return` and `echo`

- `return` exits a function with numeric status.
- `echo` outputs data.

### Difference between `[` and `[[`

- `[[` supports regex and safer comparisons.

---

# 11. Shell Script Template

```bash
#!/bin/bash

set -euo pipefail

main(){

 echo "Hello DevOps"

}

main "$@"
```

---

Happy Scripting! 🚀
