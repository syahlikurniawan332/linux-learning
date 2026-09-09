# 03 - Log Investigation and Configuration Comparison

## 📖 Introduction

In this challenge, I practiced investigating application failures using Linux troubleshooting tools.

The scenario involved analyzing application logs, inspecting kernel messages, checking web server configuration, comparing staging and production configuration files, and identifying missing files between two simulated servers.

This exercise introduced a more systematic troubleshooting workflow using Linux command-line tools.

---

## 🎯 Learning Objectives

- Search log files using `grep`.
- Filter multiple patterns with regular expressions.
- Inspect kernel messages using `dmesg`.
- Use pipes to connect commands.
- Redirect command output into files.
- Append information without overwriting existing reports.
- Compare configuration files using `diff`.
- Compare directories recursively.
- Identify inconsistencies between staging and production environments.

---

# 🔍 1. Investigating Application Logs

The application log was located at:

```text
~/project/logs/app.log
```

The goal was to find all lines containing:

```text
ERROR
```

and save them into:

```text
~/project/error_report.txt
```

I used:

```bash
grep "ERROR" ~/project/logs/app.log > ~/project/error_report.txt
```

I also tested:

```bash
grep -w "ERROR" ~/project/logs/app.log > ~/project/error_report.txt
```

Verify the result:

```bash
cat ~/project/error_report.txt
```

Output:

```text
[2023-10-26 10:00:03] ERROR: Failed to process payment transaction #12345.
[2023-10-26 10:00:05] ERROR: NullPointerException at com.innovatech.Billing.process(Billing.java:101).
```

The report now contained only the application error entries.

---

## 💡 Understanding `grep`

Basic syntax:

```bash
grep "PATTERN" FILE
```

Example:

```bash
grep "ERROR" app.log
```

The `-w` option matches the pattern as a complete word:

```bash
grep -w "ERROR" app.log
```

This can help avoid matching text where the pattern is only part of another word.

---

# 🧾 Output Redirection

The command used:

```bash
grep "ERROR" app.log > error_report.txt
```

uses:

```text
>
```

to redirect command output into a file.

Remember:

```text
>   overwrite / create
>>  append
```

This becomes important later in the investigation.

---

# 🖥️ 2. Investigating Kernel Messages

Application errors can sometimes originate from lower-level system or driver issues.

I searched the kernel message buffer using:

```bash
dmesg | grep -Ei 'fail|error' > ~/project/boot_issues.txt
```

However, the command failed:

```text
dmesg: read kernel buffer failed: Operation not permitted
```

---

## 🐛 Permission Issue

The kernel message buffer required elevated privileges.

The corrected command was:

```bash
sudo dmesg | grep -Ei 'fail|error' > ~/project/boot_issues.txt
```

Then:

```bash
cat ~/project/boot_issues.txt
```

showed messages containing words related to failures or errors.

---

## Understanding the Pipeline

This command:

```bash
sudo dmesg | grep -Ei 'fail|error'
```

contains two important concepts.

### Pipe `|`

```text
command1 | command2
```

sends the output from `command1` into `command2`.

Here:

```text
dmesg
  ↓
grep
```

`dmesg` generates kernel messages, while `grep` filters them.

---

### `grep -Ei`

The options mean:

```text
-E = extended regular expressions
-i = case-insensitive search
```

The pattern:

```text
fail|error
```

means:

```text
fail OR error
```

So the command can match:

```text
fail
failed
error
Errors
ERROR
```

regardless of capitalization.

---

# 🌐 3. Inspecting the Nginx Configuration

The next task was to inspect:

```text
~/project/config/nginx.conf
```

and locate:

```text
worker_processes
```

I used:

```bash
grep "worker_processes" ~/project/config/nginx.conf >> ~/project/error_report.txt
```

Notice that I used:

```text
>>
```

instead of:

```text
>
```

because the existing error report needed to be preserved.

Verify:

```bash
cat ~/project/error_report.txt
```

Output:

```text
[2023-10-26 10:00:03] ERROR: Failed to process payment transaction #12345.
[2023-10-26 10:00:05] ERROR: NullPointerException at com.innovatech.Billing.process(Billing.java:101).
worker_processes 4;
```

---

## 💡 Why `>>` Matters

Using:

```bash
>
```

would replace the previous report.

Using:

```bash
>>
```

adds new information to the end.

For troubleshooting reports, this allows multiple findings to be collected into one file.

---

# ⚙️ 4. Comparing Staging and Production Configuration

The next investigation focused on environment differences.

The two files were:

```text
~/project/config/staging/app.conf
~/project/config/production/app.conf
```

I compared them using:

```bash
diff ~/project/config/staging/app.conf \
     ~/project/config/production/app.conf \
     > ~/project/config_diff.txt
```

Verify:

```bash
cat ~/project/config_diff.txt
```

Example:

```text
1,5c1,5
< # Staging Configuration
< database.url=jdbc:mysql://staging-db:3306/nexus
< api.key=<staging_key>
< feature.flag.new_dashboard=true
< timeout.ms=3000
---
> # Production Configuration
> database.url=jdbc:mysql://prod-db:3306/nexus
> api.key=<production_key>
> feature.flag.new_dashboard=false
> timeout.ms=5000
```

---

## Reading `diff` Output

In standard `diff` output:

```text
<
```

represents content from the first file.

```text
>
```

represents content from the second file.

Since I ran:

```bash
diff staging production
```

the `<` lines came from staging, while the `>` lines came from production.

---

## Important Differences Found

The environments differed in several areas:

```text
Database URL
API key
Feature flag
Timeout value
```

Environment configuration differences are a common source of deployment problems.

---

## 🔐 Security Note

Configuration files can contain sensitive values such as:

```text
API keys
database credentials
tokens
passwords
```

Real secrets should never be committed to a public Git repository.

For documentation, sensitive values should be replaced with placeholders such as:

```text
<api_key>
<password>
<token>
```

---

# 📁 5. Comparing Server Directories

The final task simulated comparing files between staging and production servers.

The directories were:

```text
/home/labex/project/server1_files
/home/labex/project/server2_files
```

I used:

```bash
diff -r /home/labex/project/server1_files \
        /home/labex/project/server2_files \
        > /home/labex/project/missing_files.txt
```

Verify:

```bash
cat /home/labex/project/missing_files.txt
```

Output:

```text
Only in /home/labex/project/server1_files: asset2.js
```

This indicates that:

```text
asset2.js
```

exists on the first server but is missing from the second server.

---

## Recursive Directory Comparison

The option:

```text
-r
```

means:

```text
recursive
```

So:

```bash
diff -r dir1 dir2
```

compares not only the directories themselves, but also files and nested directories inside them.

---

# 🧭 Troubleshooting Workflow

This challenge introduced a useful investigation pattern:

```text
Application Failure
        ↓
Check application logs
        ↓
Check kernel/system messages
        ↓
Inspect service configuration
        ↓
Compare environments
        ↓
Compare deployed files
        ↓
Document findings
```

Instead of guessing the cause, each step collects evidence.

---

# 🐛 Problems Encountered During Practice

## 1. `dmesg` Permission Error

Command:

```bash
dmesg
```

returned:

```text
Operation not permitted
```

Solution:

```bash
sudo dmesg
```

Lesson:

> Some system-level information requires elevated privileges.

---

## 2. Choosing Between `>` and `>>`

For the initial report:

```bash
grep "ERROR" app.log > error_report.txt
```

For additional findings:

```bash
grep "worker_processes" nginx.conf >> error_report.txt
```

Lesson:

```text
>  replaces
>> appends
```

Choosing the wrong operator can destroy previous report contents.

---

# 🧠 Command Reference

| Command | Purpose |
|---|---|
| `grep "pattern" file` | Search text inside a file |
| `grep -w` | Match a complete word |
| `grep -i` | Case-insensitive search |
| `grep -E` | Use extended regular expressions |
| `dmesg` | Display kernel messages |
| `sudo dmesg` | Read kernel messages with elevated privileges |
| `\|` | Pipe output to another command |
| `>` | Redirect and overwrite output |
| `>>` | Append output |
| `diff file1 file2` | Compare two files |
| `diff -r dir1 dir2` | Compare directories recursively |
| `cat file` | Display file contents |

---

# 💡 Key Takeaways

- `grep` is useful for extracting relevant information from large log files.
- `dmesg` provides kernel and driver-level messages.
- Some kernel information may require `sudo`.
- Pipes allow commands to work together.
- `>` and `>>` serve different purposes when generating reports.
- `diff` is useful for detecting configuration drift.
- Configuration differences between staging and production can cause application failures.
- `diff -r` can identify files missing between server directories.
- Troubleshooting should be evidence-driven rather than based on guesses.
- Sensitive configuration values should not be exposed in public repositories.

---

## 📝 Summary

In this challenge, I investigated a simulated application failure using Linux troubleshooting tools.

I used:

```bash
grep
dmesg
diff
```

along with:

```text
|
>
>>
```

to collect and analyze information from application logs, kernel messages, web server configuration, environment configuration files, and server directories.

The exercise demonstrated how Linux command-line tools can be combined into a systematic troubleshooting workflow.

Instead of looking at only one source of information, I learned to investigate problems across multiple layers:

```text
Application
System
Configuration
Deployment
Filesystem
```

This approach is fundamental to system administration, troubleshooting, and DevOps work.