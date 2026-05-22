# Day 06 - File I/O Practice

## Objective
Practice basic Linux file input/output operations using fundamental commands.

---

## Commands Practiced

### 1. Create a File

```bash
touch notes.txt
```

Creates an empty file named `notes.txt`.

---

### 2. Write Data into File

```bash
echo "First Line" > notes.txt
```

Writes text into the file using `>` operator.  
This overwrites existing content.

---

### 3. Append Data into File

```bash
echo "Second Line" >> notes.txt
echo "Third Line" >> notes.txt
```

Appends new lines into the file using `>>`.

---

### 4. Read Full File Content

```bash
cat notes.txt
```

Displays the complete file content.

Output:

```text
First Line
Second Line
Third Line
```

---

### 5. Read First Few Lines

```bash
head -n 2 notes.txt
```

Displays the first 2 lines from the file.

Output:

```text
First Line
Second Line
```

---

### 6. Read Last Few Lines

```bash
tail -n 2 notes.txt
```

Displays the last 2 lines from the file.

Output:

```text
Second Line
Third Line
```

---

### 7. Use tee Command

```bash
echo "Fourth Line" | tee -a notes.txt
```

The `tee` command displays output on the terminal and appends it to the file at the same time.

Output:

```text
Fourth Line
```

---

## Final File Content

```bash
cat notes.txt
```

Output:

```text
First Line
Second Line
Third Line
Fourth Line
```

---

## What I Learned

- Creating files using `touch`
- Writing and appending data using `>` and `>>`
- Reading files using `cat`
- Viewing partial file content using `head` and `tail`
- Using `tee` to display and write output simultaneously

---

## Why This Matters in DevOps

File operations are important in DevOps because logs, configuration files, and scripts are all stored as text files.  
Understanding file handling helps in automation, troubleshooting, and server management.