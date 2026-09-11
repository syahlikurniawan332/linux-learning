# 04 - The Fortress Guardian: Permissions and Collaboration

## 📖 Introduction

In this challenge, I practiced securing Project Phoenix using Linux ownership and permission controls.

The scenario focused on protecting sensitive files, assigning project ownership to the correct user and group, restricting access to the main project directory, and configuring a collaborative development directory using the `setgid` bit.

---

## 🎯 Learning Objectives

- Protect sensitive files with strict permissions.
- Understand numeric permission notation.
- Change file and directory ownership recursively.
- Configure directory access for owner, group, and others.
- Understand execute permission on directories.
- Use the `setgid` bit for collaborative directories.
- Verify group inheritance on newly created files.

---

# 🔐 1. Creating a Secure Project File

The first task was to create a sensitive file:

```text
~/project/phoenix_project/project_keys.txt
```

Create the file:

```bash
touch ~/project/phoenix_project/project_keys.txt
```

Verify:

```bash
ls ~/project/phoenix_project
```

Output:

```text
docs  project_keys.txt  src
```

Check the initial permissions:

```bash
ls -l ~/project/phoenix_project/project_keys.txt
```

Output:

```text
-rw-rw-r-- 1 labex labex 0 Sep 11 16:06 project_keys.txt
```

The initial permission allowed access beyond the owner, so it needed to be restricted.

---

## Initial Attempt: `700`

I first used:

```bash
chmod 700 ~/project/phoenix_project/project_keys.txt
```

Result:

```text
-rwx------ 1 labex labex 0 Sep 11 16:06 project_keys.txt
```

`700` gives:

```text
Owner  : rwx
Group  : ---
Others : ---
```

However, this file only needed read and write access.

The execute permission was unnecessary.

---

## Correct Permission: `600`

I changed the permission to:

```bash
chmod 600 ~/project/phoenix_project/project_keys.txt
```

Verify:

```bash
ls -l ~/project/phoenix_project/project_keys.txt
```

Output:

```text
-rw------- 1 labex labex 0 Sep 11 16:06 project_keys.txt
```

`600` means:

```text
Owner  : rw-
Group  : ---
Others : ---
```

Only the file owner can read and modify the file.

---

## 💡 Why `600` Instead of `700`?

Numeric permissions are calculated using:

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
6 = 4 + 2 = rw-
```

For a sensitive data file:

```text
600
```

is more appropriate than:

```text
700
```

because the file does not need to be executable.

---

# 👤 2. Assigning Project Ownership

The entire Project Phoenix directory needed to be owned by:

```text
User  : dev_lead
Group : developers
```

I used:

```bash
sudo chown -R dev_lead:developers ~/project/phoenix_project
```

The `-R` option applies the ownership change recursively.

---

## Verify the Main Directory

```bash
ls -ld ~/project/phoenix_project/
```

Output:

```text
drwxrwxr-x 4 dev_lead developers 53 Sep 11 16:06 /home/labex/project/phoenix_project/
```

---

## Verify the Contents

```bash
ls -l ~/project/phoenix_project/
```

Output:

```text
total 0
drwxrwxr-x 2 dev_lead developers 27 Sep 11 16:05 docs
-rw------- 1 dev_lead developers  0 Sep 11 16:06 project_keys.txt
drwxrwxr-x 2 dev_lead developers  6 Sep 11 16:05 src
```

All project resources now belong to:

```text
dev_lead:developers
```

---

# 🛡️ 3. Securing the Main Project Directory

The required access policy was:

```text
Owner  : rwx
Group  : r-x
Others : ---
```

This corresponds to:

```text
750
```

I applied:

```bash
sudo chmod 750 ~/project/phoenix_project
```

Verify:

```bash
ls -ld ~/project/phoenix_project/
```

Output:

```text
drwxr-x--- 4 dev_lead developers 53 Sep 11 16:06 /home/labex/project/phoenix_project/
```

---

## Understanding `750`

```text
7 = rwx
5 = r-x
0 = ---
```

So:

```text
Owner  : read + write + execute
Group  : read + execute
Others : no access
```

---

## 💡 Execute Permission on Directories

The meaning of `x` is different for directories.

For a directory:

```text
r = list directory contents
w = create/delete entries
x = enter/traverse the directory
```

So the `developers` group needs:

```text
r-x
```

to list and enter the project directory.

---

## 🐛 Path Mistake

I accidentally checked:

```bash
ls -ld ~/project/phoenix_project/~
```

Output:

```text
ls: cannot access '/home/labex/project/phoenix_project/~': No such file or directory
```

The correct command was:

```bash
ls -ld ~/project/phoenix_project/
```

This reinforced that `~` only expands as the home directory when it appears in the correct shell context, not as an arbitrary path component.

---

# 👥 4. Creating a Collaborative Development Directory

The `src` directory needed to support team collaboration.

The requirement was:

- Owner can read, write, and enter.
- Group can read, write, and enter.
- Others have no access.
- New files automatically inherit the `developers` group.

This requires the `setgid` bit.

---

## Applying `setgid`

I used:

```bash
sudo chmod 2770 ~/project/phoenix_project/src
```

Verify:

```bash
ls -ld ~/project/phoenix_project/src
```

Output:

```text
drwxrws--- 2 dev_lead developers 6 Sep 11 16:05 /home/labex/project/phoenix_project/src
```

Notice:

```text
rws
```

in the group permissions.

The lowercase:

```text
s
```

shows that:

- group execute permission exists
- the `setgid` bit is enabled

---

# 🔢 Understanding `2770`

The first digit:

```text
2
```

represents:

```text
setgid
```

The remaining digits are normal permissions:

```text
770
```

which means:

```text
Owner  : rwx
Group  : rwx
Others : ---
```

Therefore:

```text
2770
```

means:

```text
setgid + rwxrwx---
```

---

# 🧪 Testing Group Inheritance

To verify the behavior, I created a new file:

```bash
touch ~/project/phoenix_project/src/new_file.txt
```

Then checked its ownership:

```bash
ls -l ~/project/phoenix_project/src/new_file.txt
```

Output:

```text
-rw-rw-r-- 1 labex developers 0 Sep 11 16:25 new_file.txt
```

The file owner is:

```text
labex
```

but the group is automatically:

```text
developers
```

This confirms that the `setgid` behavior worked correctly.

---

# 🧠 Why `setgid` Is Useful

Without `setgid`, a newly created file normally inherits the creator's default group.

With `setgid` enabled on a directory:

```text
new files
    ↓
inherit directory group
    ↓
developers
```

This is useful for shared development directories where multiple team members need consistent group ownership.

---

# 🔄 User Ownership vs Group Ownership

The newly created file showed:

```text
labex developers
```

This demonstrates an important distinction:

```text
User owner
→ remains the account that created the file

Group owner
→ inherited from the setgid directory
```

`setgid` does not force the user owner to become `dev_lead`.

It only affects group inheritance.

---

# 🧭 Security Model Built in This Challenge

The final structure can be understood as:

```text
phoenix_project/
│
├── project_keys.txt
│   └── 600
│       owner only
│
├── docs/
│
└── src/
    └── 2770
        owner + developers
        setgid enabled
```

And the project directory itself:

```text
phoenix_project
└── 750
    owner     → full access
    developers → read + enter
    others     → no access
```

---

# 🐛 Important Lessons from Practice

## 1. `700` Was Too Permissive for the File Owner

I initially used:

```bash
chmod 700 project_keys.txt
```

This added execute permission unnecessarily.

The correct permission was:

```bash
chmod 600 project_keys.txt
```

Lesson:

> Permissions should provide only the access actually required.

---

## 2. Recursive Ownership

Using:

```bash
sudo chown -R dev_lead:developers ~/project/phoenix_project
```

changed ownership for the directory and everything below it.

Lesson:

> Be careful with `-R`, because recursive changes can affect many files at once.

---

## 3. `setgid` Controls Group Inheritance

Using:

```bash
chmod 2770 src
```

made new files inherit:

```text
developers
```

instead of the creator's default group.

Lesson:

> Special permission bits can change filesystem behavior beyond normal `rwx` permissions.

---

# 🧠 Command Reference

| Command | Purpose |
|---|---|
| `touch file` | Create an empty file |
| `ls -l` | Inspect file permissions and ownership |
| `ls -ld` | Inspect a directory itself |
| `chmod 600 file` | Owner read/write only |
| `chmod 750 dir` | Owner full, group read/execute, others none |
| `chmod 2770 dir` | Enable setgid with `rwxrwx---` |
| `chown user:group` | Change owner and group |
| `chown -R` | Change ownership recursively |
| `sudo` | Execute commands with elevated privileges |

---

# 💡 Key Takeaways

- File permissions should follow the principle of least privilege.
- `600` is suitable for sensitive files that only the owner should read or modify.
- `chown -R` changes ownership recursively.
- Directory permissions behave differently from file permissions.
- `x` on a directory allows traversal.
- `750` protects a directory from unauthorized users while allowing group access.
- `setgid` on a directory causes new files to inherit its group.
- `2770` combines `setgid` with collaborative `rwxrwx---` permissions.
- User ownership and group ownership are separate concepts.
- Permission and ownership changes should always be verified with `ls -l` or `ls -ld`.

---

## 📝 Summary

In this challenge, I practiced building a secure and collaborative Linux project workspace.

I secured a sensitive file using:

```bash
chmod 600
```

transferred project ownership with:

```bash
chown -R
```

restricted the main project directory using:

```bash
chmod 750
```

and configured the development directory using:

```bash
chmod 2770
```

The most important new concept was the `setgid` bit.

By enabling `setgid` on the `src` directory, newly created files automatically inherited the `developers` group while keeping the actual user who created the file as the owner.

This provides a practical balance between:

```text
Security
+
Access Control
+
Team Collaboration
```

which is important in Linux system administration and shared development environments.