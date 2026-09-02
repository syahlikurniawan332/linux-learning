# 03 - File Content and Comparing

## 📖 Introduction

In this lesson, I learned how to inspect file contents, display specific parts of a file, and compare files and directories in Linux.

These commands are useful when checking configuration files, logs, source code, or differences between multiple versions of a file.

---

## 🎯 Learning Objectives

- Display the entire content of a file.
- Display file contents with line numbers.
- View the beginning and end of a file.
- Read file content by lines or bytes.
- Compare two files.
- Compare directories recursively.

---

## 📚 Commands

### `cat`

Displays the entire content of a file.

```bash
cat /tmp/hello
```

Example output:

```text
Hi,
I am Labby!
```

---

### `cat -n`

Displays the file content with line numbers.

```bash
cat -n /tmp/hello
```

Example output:

```text
1  Hi,
2  I am Labby!
```

This is useful when inspecting files where specific line numbers are important.

---

### `head`

Displays the beginning of a file.

Display the first line:

```bash
head -n1 /tmp/hello
```

Output:

```text
Hi,
```

The `-n` option specifies the number of lines to display.

---

### `head -c`

Displays the beginning of a file based on bytes instead of lines.

```bash
head -c1 /tmp/hello
```

Output:

```text
H
```

The `-c` option specifies the number of bytes to display.

---

### `tail`

Displays the end of a file.

```bash
tail -n1 /tmp/hello
```

Output:

```text
I am Labby!
```

---

### `tail -c`

Displays the end of a file based on bytes.

```bash
tail -c2 /tmp/hello
```

Output:

```text
!
```

---

### `diff`

Compares the contents of two files and displays their differences.

```bash
diff file1 file2
```

Example output:

```text
1c1
< this is file1
---
> this is file2
```

This output shows that line 1 in `file1` is different from line 1 in `file2`.

---

### `diff -r`

Compares two directories recursively.

```bash
diff -r ~/Desktop ~/Code
```

Example output:

```text
Only in /home/labex/Desktop: code.desktop
Only in /home/labex/Desktop: gedit.desktop
Only in /home/labex/Desktop: gvim.desktop
Only in /home/labex/Desktop: xfce4-terminal.desktop
```

The `-r` option compares the contents of directories recursively.

---

## 🧪 Practice - The Manuscript Mystery

In this exercise, I compared two versions of a manuscript to identify what had changed.

### Inspect the First File

```bash
cat /home/labex/project/manuscript_v1.txt
```

Output:

```text
The old clock struck midnight.
A shadow moved across the room.
The detective's eyes narrowed.
```

---

### Compare the Beginning of Both Files

```bash
head -n2 /home/labex/project/manuscript_v1.txt
```

Output:

```text
The old clock struck midnight.
A shadow moved across the room.
```

```bash
head -n2 /home/labex/project/manuscript_v2.txt
```

Output:

```text
The old clock struck midnight.
A figure darted behind the curtains.
```

The difference appears on the second line.

---

### Compare the End of Both Files

```bash
tail -n1 /home/labex/project/manuscript_v1.txt
```

```text
The detective's eyes narrowed.
```

```bash
tail -n1 /home/labex/project/manuscript_v2.txt
```

```text
The detective's eyes narrowed.
```

The last line of both files is identical.

---

### Compare the Files with `diff`

At first, I used the wrong filenames:

```bash
diff manuscript_v1 manuscript_v2
```

Output:

```text
diff: manuscript_v1: No such file or directory
diff: manuscript_v2: No such file or directory
```

The files actually use the `.txt` extension.

Using the correct filenames:

```bash
diff manuscript_v1.txt manuscript_v2.txt
```

Output:

```text
2c2
< A shadow moved across the room.
---
> A figure darted behind the curtains.
```

This shows that line 2 was changed between the two manuscript versions.

---

## 💡 Key Takeaways

- `cat` displays the entire contents of a file.
- `cat -n` adds line numbers to the output.
- `head` displays the beginning of a file.
- `tail` displays the end of a file.
- The `-n` option works with lines.
- The `-c` option works with bytes.
- `diff` compares the contents of two files.
- `diff -r` recursively compares directories.
- File names must be written correctly, including their extensions.

---

## 📝 Summary

In this lesson, I practiced inspecting and comparing files using `cat`, `head`, `tail`, and `diff`.

I also learned that errors such as `No such file or directory` can occur simply because the filename or extension is incorrect. Checking the exact filename before running a command is an important part of working with Linux.

If I forget how a Linux command works, I can use the manual pages:

```bash
man cat
man head
man tail
man diff
```