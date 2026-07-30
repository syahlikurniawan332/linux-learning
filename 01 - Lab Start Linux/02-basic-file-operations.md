# 02 - Basic File Operations

## 📖 Introduction

In this lesson, I learned the basic Linux commands used to navigate the file system and manage files and directories. These commands are fundamental because almost every task in Linux involves working with files and folders.

---

## 🎯 Learning Objectives

- Navigate the Linux file system.
- Create files and directories.
- List directory contents.
- Copy files and directories.
- Move and rename files.
- Delete files and directories safely.

---

## 📚 Commands

### `pwd`

Displays the current working directory.

```bash
pwd
```

---

### `echo ~`

Displays the path to the current user's home directory.

```bash
echo ~
```

---

### `ls`

Lists files and directories in the current location.

```bash
ls
```

Useful options:

```bash
ls -l
ls -a
ls -la
ls ~
ls -l testdir
```

---

### `touch`

Creates an empty file.

```bash
touch file1.txt
```

---

### `echo`

Writes text into a file.

```bash
echo "Hello, Linux" > file2.txt
```

Create a hidden file.

```bash
echo "Hidden file" > .hiddenfile
```

---

### `mkdir`

Creates a new directory.

```bash
mkdir testdir
```

---

### `cp`

Copies files or directories.

Copy a file.

```bash
cp file1.txt file1_copy.txt
```

Copy a file into another directory.

```bash
cp file2.txt testdir/
```

Copy an entire directory.

```bash
cp -r testdir testdir_copy
```

---

### `mv`

Moves or renames files and directories.

```bash
mv oldname.txt newname.txt
```

---

### `rm`

Removes files.

```bash
rm original_file1.txt
```

Ask for confirmation before deleting.

```bash
rm -i file2.txt
```

Remove a directory and its contents.

```bash
rm -r testdir
```

Force remove without confirmation.

```bash
rm -rf testdir
```

---

### `rmdir`

Removes an empty directory.

```bash
rmdir new_testdir
```

---

## ⚠️ Danger Zone

The `rm -rf` command permanently removes files and directories without sending them to a recycle bin.

Always double-check the target path before running this command.

Example:

```bash
rm -rf /
```

Running this command with sufficient privileges can seriously damage or completely destroy the operating system.

---

## 💡 Key Takeaways

- Linux uses a hierarchical file system starting from the root directory (`/`).
- `pwd` displays the current working directory.
- `ls` lists files and directories with different viewing options.
- `touch` and `mkdir` create files and directories.
- `cp` copies files and directories.
- `mv` moves or renames files.
- `rm` and `rmdir` remove files and directories.
- `rm -rf` is one of the most dangerous Linux commands and should be used carefully.

---

## 📝 Summary

- Navigated the Linux file system using `pwd`.
- Listed directory contents using `ls`.
- Created files and directories with `touch` and `mkdir`.
- Copied files and directories using `cp`.
- Moved and renamed files using `mv`.
- Removed files and directories using `rm` and `rmdir`.
- Learned the risks of using `rm -rf`.