# 02 - File Organization, Backup, and Archiving

## 📖 Introduction

In this challenge, I practiced organizing a Linux project directory, moving files into appropriate locations, creating backups, relocating shared resources, and archiving old log files.

The scenario focused on reorganizing the Project Phoenix environment into a cleaner and more maintainable structure.

---

## 🎯 Learning Objectives

- Create multiple directories using `mkdir`.
- Move files and directories using `mv`.
- Copy files using `cp`.
- Organize project files into logical directories.
- Create compressed archives using `tar`.
- Use wildcards to select multiple files.
- Remove files after successful archiving.
- Understand common mistakes when working with paths and command options.

---

# 📁 1. Organizing the Project Structure

The project needed separate directories for:

```text
src
config
docs
```

The intended structure was:

```text
phoenix_project/
├── config/
├── docs/
└── src/
```

These directories help separate source code, configuration files, and documentation.

---

# 📦 2. Moving Project Files

The project files were moved into their appropriate directories:

```text
main_app.py  → src/
config.json  → config/
README.md    → docs/
```

The `mv` command is used to move files or rename them.

Basic syntax:

```bash
mv SOURCE DESTINATION
```

For example:

```bash
mv main_app.py src/
```

---

# 💾 3. Creating a Configuration Backup

Before modifying important configuration files, a backup should be created.

The configuration file:

```text
config.json
```

was copied to:

```text
config.json.bak
```

using:

```bash
cp config.json config.json.bak
```

This preserves the original configuration in case a future change causes problems.

---

# 📂 4. Moving Shared Documentation

The `shared_docs` directory needed to be moved into:

```text
phoenix_project/docs/
```

The directory contained:

```text
api_spec.doc
team_guidelines.txt
```

---

## 🐛 Mistake: Using `mv -r`

I initially tried:

```bash
mv -r shared_docs phoenix_project/docs/
```

Output:

```text
mv: invalid option -- 'r'
Try 'mv --help' for more information.
```

Unlike `cp`, the `mv` command does not require `-r` to move directories.

The correct syntax is simply:

```bash
mv shared_docs phoenix_project/docs/
```

---

## Directory Already Existed

When I tried:

```bash
mv shared_docs phoenix_project/docs/
```

I received:

```text
mv: cannot move 'shared_docs' to 'phoenix_project/docs/shared_docs': File exists
```

This indicated that a `shared_docs` directory already existed in the destination.

I then removed the remaining source directory:

```bash
rm -r shared_docs
```

Afterward:

```bash
ls
```

showed:

```text
logs  phoenix_project
```

---

# 🔎 Verifying the Documentation Directory

I entered the project:

```bash
cd phoenix_project
```

Then I accidentally tried:

```bash
ls /docs
```

Output:

```text
ls: cannot access '/docs': No such file or directory
```

The problem was the leading `/`.

```text
/docs
```

means a directory named `docs` directly under the filesystem root.

The correct relative path was:

```bash
ls docs
```

Output:

```text
README.md  shared_docs
```

Then:

```bash
ls docs/shared_docs
```

Output:

```text
api_spec.doc  team_guidelines.txt
```

---

## 💡 Relative vs Absolute Paths

This mistake helped reinforce the difference between:

```text
docs
```

and:

```text
/docs
```

`docs` is relative to the current directory.

`/docs` refers to a directory directly under the Linux root filesystem.

---

# 🗃️ 5. Inspecting Log Files

I moved to the logs directory:

```bash
cd ~/project/logs
```

Then checked the files:

```bash
ls
```

Output:

```text
app_2023-01-15.log
app_2024-05-01.log
db_2023-02-20.log
```

The goal was to archive only the logs from 2023.

---

# 🗜️ 6. Creating a Compressed Archive

The `tar` command can combine multiple files into one archive.

The options used were:

```text
c = create archive
z = compress using gzip
f = specify archive filename
```

So:

```bash
tar -czf archive.tar.gz file1 file2
```

means:

```text
Create a gzip-compressed archive named archive.tar.gz
containing file1 and file2
```

---

## 🐛 Mistake: Archiving Nonexistent Files

I initially tried:

```bash
tar -czf archive.tar.gz file1 file2
```

Output:

```text
tar: file1: Cannot stat: No such file or directory
tar: file2: Cannot stat: No such file or directory
tar: Exiting with failure status due to previous errors
```

The files `file1` and `file2` did not exist.

This showed that the archive command must reference actual files.

---

# ⭐ Selecting Files with Wildcards

To find only the 2023 log files:

```bash
ls *_2023-*.log
```

Output:

```text
app_2023-01-15.log
db_2023-02-20.log
```

The wildcard:

```text
*_2023-*.log
```

matches log files containing `_2023-` in their names.

---

# 📦 Creating the Correct Archive

I created the required archive:

```bash
tar -czf old_logs.tar.gz *_2023-*.log
```

Then verified its contents:

```bash
tar -tzf old_logs.tar.gz
```

Output:

```text
app_2023-01-15.log
db_2023-02-20.log
```

The options used here were:

```text
t = list archive contents
z = gzip archive
f = archive filename
```

---

# 🗑️ Removing the Archived Logs

After confirming that the archive contained the correct files, I removed the original 2023 logs:

```bash
rm -- *_2023-*.log
```

The `--` tells `rm` that everything after it should be treated as a filename or pattern rather than as a command option.

---

## Final Directory Contents

After removing the old logs:

```bash
ls
```

Output:

```text
app_2024-05-01.log
archive.tar.gz
old_logs.tar.gz
```

The important result is that:

```text
app_2024-05-01.log
```

remained untouched, while the 2023 logs were archived and removed.

---

## ⚠️ Leftover Failed Archive

The file:

```text
archive.tar.gz
```

was created during my earlier failed `tar` attempt.

Because the source files did not exist, this file was not part of the required task.

In a real environment, I should inspect or remove unnecessary artifacts created by failed commands.

For example:

```bash
rm archive.tar.gz
```

after confirming that it is not needed.

---

# 🧠 Command Reference

| Command | Purpose |
|---|---|
| `mkdir` | Create directories |
| `mv` | Move or rename files/directories |
| `cp` | Copy files |
| `ls` | List directory contents |
| `tar -czf` | Create gzip-compressed archive |
| `tar -tzf` | List archive contents |
| `rm` | Remove files |
| `rm -r` | Remove directories recursively |
| `*` | Wildcard for matching filenames |

---

## 💡 Key Takeaways

- `mkdir` can create multiple directories at once.
- `mv` moves directories without requiring `-r`.
- `cp` is useful for creating backups.
- A leading `/` changes a path into an absolute path.
- `tar -czf` creates a compressed archive.
- `tar -tzf` verifies archive contents.
- Wildcards can target groups of files.
- Archives should be verified before deleting original files.
- Failed commands can sometimes leave unwanted files behind.
- File organization is an important part of Linux system administration.

---

## 📝 Summary

In this challenge, I practiced organizing Project Phoenix into a cleaner directory structure.

I worked with:

```bash
mkdir
mv
cp
tar
rm
```

I also learned several practical lessons from mistakes during the lab.

First, `mv` does not use the recursive `-r` option when moving directories.

Second, I reinforced the difference between relative paths such as:

```text
docs/
```

and absolute paths such as:

```text
/docs/
```

Finally, I practiced creating compressed archives with `tar`, selecting files with wildcards, verifying archive contents, and safely removing old files after confirming that they had been archived.

These skills are useful for project organization, backup management, system maintenance, and log cleanup.