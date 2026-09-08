# 07 - System Information and Output Redirection

## 📖 Introduction

In this challenge, I practiced inspecting basic Linux system information and creating a simple system report using output redirection.

The commands helped me identify the current user, inspect the Linux kernel and system details, check system uptime and load averages, and save command output into a text file.

---

## 🎯 Learning Objectives

- Identify the current Linux user.
- Display the operating system kernel name.
- Display detailed system and kernel information.
- Check system uptime and load averages.
- Understand output redirection using `>` and `>>`.
- Save command output into a system report file.

---

# 👤 Checking the Current User

I first created a file for the system report:

```bash
touch system_report.txt
```

Then I accidentally tried:

```bash
whoami system_report.txt
```

Output:

```text
whoami: extra operand 'system_report.txt'
Try 'whoami --help' for more information.
```

The `whoami` command does not take a filename as an argument.

The correct command was:

```bash
whoami
```

Output:

```text
labex
```

This shows that the current user is:

```text
labex
```

---

# 🐧 Checking the Kernel

Display the kernel name:

```bash
uname
```

Output:

```text
Linux
```

This confirms that the system is running the Linux kernel.

---

## Detailed System Information

To display more complete system information:

```bash
uname -a
```

Output:

```text
Linux 6a9f8921d5fc9a7cba568680 6.8.0-111-generic #111-Ubuntu SMP PREEMPT_DYNAMIC Sat Apr 11 23:16:02 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
```

`uname -a` provides information such as:

```text
Kernel name
Hostname
Kernel release
Kernel version
System architecture
Operating system
```

---

# ⏱️ Checking System Uptime

I used:

```bash
uptime
```

Output:

```text
12:10:41 up 118 days, 2:10, 0 users, load average: 0.14, 0.28, 0.23
```

The output includes:

- Current system time
- How long the system has been running
- Number of logged-in users
- System load averages

The three load average values represent approximately:

```text
1 minute
5 minutes
15 minutes
```

---

# 📝 Creating a System Report

The challenge required command output to be saved into:

```text
system_report.txt
```

Linux provides output redirection operators for this.

---

## `>` — Write / Overwrite

I used:

```bash
whoami > system_report.txt
```

Then checked the file:

```bash
cat system_report.txt
```

Output:

```text
labex
```

The `>` operator writes command output into a file.

If the file already contains data, the existing content is overwritten.

---

## `>>` — Append

To add information without deleting the existing content, I used:

```bash
uptime >> system_report.txt
```

and:

```bash
uname -a >> system_report.txt
```

The `>>` operator appends new output to the end of the file.

---

## Final Report

Check the report:

```bash
cat system_report.txt
```

Output:

```text
labex
12:13:45 up 118 days, 2:13, 0 users, load average: 0.24, 0.26, 0.23
Linux 6a9f8921d5fc9a7cba568680 6.8.0-111-generic #111-Ubuntu SMP PREEMPT_DYNAMIC Sat Apr 11 23:16:02 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
```

The report now contains information about:

```text
Current user
System uptime
Kernel and system information
```

---

# 🐛 Important Observation: `>` Overwrites Data

During the practice, I initially ran:

```bash
whoami > system_report.txt
uname -a >> system_report.txt
uptime >> system_report.txt
```

But afterward, I ran:

```bash
whoami > system_report.txt
```

again.

Because `>` overwrites the file, the previous `uname -a` and `uptime` output was removed.

I then added the information again using:

```bash
uptime >> system_report.txt
uname -a >> system_report.txt
```

This helped me understand the practical difference between:

```text
>   overwrite
>>  append
```

---

# 💡 `>` vs `>>`

| Operator | Purpose |
|---|---|
| `>` | Write output and overwrite existing file contents |
| `>>` | Append output without deleting existing contents |

Example:

```bash
whoami > report.txt
```

creates or replaces the file.

Then:

```bash
uname -a >> report.txt
uptime >> report.txt
```

adds additional information.

---

# 📌 Additional Commands Covered in the Challenge

The challenge also introduces several useful system inspection commands.

## `id`

Displays UID, GID, and group membership:

```bash
id
```

Useful for understanding the current user's identity and permissions.

---

## `top`

Provides real-time system monitoring:

```bash
top
```

It can display:

- CPU usage
- Memory usage
- Running processes
- Process IDs
- System load
- Process owners

Press:

```text
q
```

to exit `top`.

---

## `who`

Displays currently logged-in users:

```bash
who
```

This can be useful when checking who else is currently using a Linux system.

---

# 🧠 Command Reference

| Command | Purpose |
|---|---|
| `whoami` | Display current username |
| `uname` | Display kernel name |
| `uname -a` | Display detailed system information |
| `uptime` | Display uptime and load averages |
| `id` | Display user and group identifiers |
| `who` | Display logged-in users |
| `top` | Monitor processes and system resources |
| `cat file` | Display file contents |
| `>` | Redirect output and overwrite file |
| `>>` | Append output to a file |

---

## 💡 Key Takeaways

- `whoami` identifies the current user.
- `uname` displays the kernel name.
- `uname -a` provides detailed system information.
- `uptime` shows how long the system has been running and its load averages.
- `>` writes output to a file and replaces existing content.
- `>>` adds output without removing existing content.
- A command cannot accept arbitrary arguments unless its syntax supports them.
- System information can be combined into a text report using output redirection.
- Commands such as `id`, `who`, and `top` are useful for system administration and monitoring.

---

## 📝 Summary

In this challenge, I practiced collecting basic Linux system information and storing it in a report.

The main commands I used were:

```bash
whoami
uname
uname -a
uptime
```

I also practiced output redirection using:

```text
>
>>
```

The most important lesson from this exercise was understanding that:

```text
>  replaces existing file contents
>> appends new content
```

This is useful when creating logs, reports, scripts, and other automated system administration workflows.