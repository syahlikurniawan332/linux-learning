# Become a Junior System Administrator

This module focuses on practical Linux system administration tasks through a series of scenario-based challenges.

The course simulates tasks that a junior system administrator may encounter when working with Linux systems, including system inspection, filesystem management, troubleshooting, permissions, users, processes, networking, software management, backups, and automation.

---

## 📚 Module Structure

### Week 1 - Linux Foundations and Access

### Day 01 - The Lay of the Land

Inspecting the Linux environment and collecting basic system information.

Topics covered:

- Current user identification
- Kernel and system information
- System uptime and load average
- User and group information
- Basic system monitoring
- Output redirection using `>` and `>>`
- Creating a system status report

📄 [View Notes](./01-system-information-and-output-redirection.md)

---

### Day 02 - The Digital Architect

Organizing project files and maintaining a clean Linux filesystem.

Topics covered:

- Creating project directory structures
- Moving files and directories with `mv`
- Creating configuration backups with `cp`
- Working with relative and absolute paths
- Creating compressed archives with `tar`
- Selecting files using wildcards
- Verifying archive contents
- Removing archived log files safely

📄 [View Notes](./02-file-organization-backup-and-archiving.md)

---

### Day 03 - The Log Investigator

Investigating application and system problems using Linux troubleshooting tools.

Topics covered:

- Filtering application logs with `grep`
- Searching multiple patterns
- Inspecting kernel messages with `dmesg`
- Using pipes to combine commands
- Building reports with output redirection
- Inspecting Nginx configuration
- Comparing staging and production configurations
- Comparing directories recursively with `diff -r`
- Identifying missing deployment files

📄 [View Notes](./03-log-investigation-and-config-comparison.md)

---

### Day 04 - The Fortress Guardian

Securing project resources and configuring collaborative access using Linux permissions.

Topics covered:

- Securing sensitive files with `chmod 600`
- Understanding numeric permissions
- Changing ownership recursively with `chown -R`
- Configuring project directory permissions with `chmod 750`
- Understanding directory execute permissions
- Using the `setgid` bit
- Configuring collaborative directories with `chmod 2770`
- Verifying automatic group inheritance

📄 [View Notes](./04-fortress-guardian-permissions-and-collaboration.md)

---

### Day 05 - The Keeper of the Keys

Upcoming lab.

---

## Week 2 - System Operations and Automation

### Day 06 - The Process Overseer

Upcoming lab.

---

### Day 07 - The Network Navigator

Upcoming lab.

---

### Day 08 - The Software Steward

Upcoming lab.

---

### Day 09 - The Backup Sentinel

Upcoming lab.

---

### Day 10 - The Script Artisan

Upcoming lab.

---

## 🎯 Module Goals

By completing this module, I aim to:

- Practice real-world Linux system administration tasks.
- Understand how to inspect and monitor Linux systems.
- Organize and maintain Linux filesystems.
- Improve troubleshooting and log analysis skills.
- Understand Linux permissions, ownership, and access control.
- Manage users, groups, and collaborative workspaces.
- Learn Linux process and resource management.
- Build basic networking knowledge.
- Practice software and backup management.
- Build automation skills using shell scripting.
- Strengthen my Linux foundation for DevOps.

---

## ✅ Progress

### Week 1

- [x] Day 01 - The Lay of the Land
- [x] Day 02 - The Digital Architect
- [x] Day 03 - The Log Investigator
- [x] Day 04 - The Fortress Guardian
- [ ] Day 05 - The Keeper of the Keys

### Week 2

- [ ] Day 06 - The Process Overseer
- [ ] Day 07 - The Network Navigator
- [ ] Day 08 - The Software Steward
- [ ] Day 09 - The Backup Sentinel
- [ ] Day 10 - The Script Artisan

---

## 🧠 Skills Practiced So Far

So far, this module has introduced and reinforced:

```text
System Inspection
Filesystem Organization
Log Analysis
Troubleshooting
Output Redirection
Pipelines
File Comparison
Permissions
Ownership
Access Control
Collaborative Directories
```

Commands practiced include:

```bash
whoami
uname
uptime
id
top

mkdir
mv
cp
rm
tar

grep
dmesg
diff

chmod
chown
touch
ls
```

---

## 🔁 Learning Approach

My goal is not only to complete the guided challenges, but also to repeat important exercises without relying on the tutorial.

For each topic, I try to follow this process:

```text
Learn
  ↓
Practice
  ↓
Encounter Errors
  ↓
Understand the Cause
  ↓
Repeat Without Guidance
  ↓
Document
```

This repository may continue to be updated as my understanding improves through repeated practice.

---

## 📝 Notes

Each completed challenge is documented in a separate Markdown file containing:

- Commands used
- Terminal output
- Explanations
- Mistakes encountered
- Troubleshooting steps
- Key takeaways
- Practical observations

Upcoming labs will be documented as I complete them.
